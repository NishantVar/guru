---
name: ingest-channel
description: Ingest an entire YouTube channel — discover videos, apply filters, batch-download captions, and create wiki pages with progress tracking and resumability.
---

# Ingest Channel

Batch-ingest a YouTube channel into the wiki. Discovers all videos, lets the user filter, downloads captions serially, then processes each video through `/ingest` one at a time.

## Input

The user provides a YouTube channel URL, name, or handle (e.g. `@3blue1brown`, `https://www.youtube.com/@3blue1brown`).

## File I/O

All markdown reads and writes inside `gurukul/` go through the `obsidian` CLI against the `the_new_order` vault (see `/ingest`'s "File I/O" section for command shapes). JSON manifests, VTT subtitles, and `/tmp/` scratch files use normal tools (`Write`, `Edit`, `Bash`). If `obsidian` is unreachable, silently fall back to `Write`/`Edit`.

## Phase 1: Discovery

### 1a. Normalize URL

If the user gives a handle or name, construct the full URL:
```
https://www.youtube.com/@<handle>/videos
```

### 1b. Check for Existing Manifest

Look for an existing manifest in `gurukul/raw/channel-manifests/`:
```bash
ls gurukul/raw/channel-manifests/<channel-name>.json 2>/dev/null
```

If found, read it and show status:
```
Found existing ingest for @channel: X done, Y pending, Z skipped, W failed. Resume?
```
- **Yes** → skip to Phase 3, continue from pending/downloaded videos
- **No** → archive old manifest with timestamp suffix, start fresh

### 1c. Fetch Video List

```bash
/opt/homebrew/bin/yt-dlp --flat-playlist -j --skip-download --extractor-args "youtubetab:approximate_date" "<channel_url>/videos" 2>/dev/null
```

Save raw output to `gurukul/raw/channel-manifests/<channel-name>-discovery.jsonl`.

### 1d. Present Summary (Tiered Display)

- **< 20 videos**: Full list — title, date, duration, views for each
- **20–100 videos**: Yearly breakdown table + top 10 by views + 10 most recent
- **100+ videos**: Stats only (total count, date range, yearly table). Ask user to apply filters.

## Phase 2: Filtering

### 2a. Collect Filters

Ask the user (via AskUserQuestion) which filters to apply. All are optional:

| Filter | Format | Example |
|--------|--------|---------|
| `dateafter` | YYYYMMDD | `20230101` |
| `datebefore` | YYYYMMDD | `20240101` |
| `title_contains` | case-insensitive regex | `tutorial\|guide` |
| `title_excludes` | case-insensitive regex | `shorts\|livestream` |
| `min_views` | integer | `1000` |
| `max_count` | integer (cap results) | `50` |
| `sort` | `newest_first`, `oldest_first`, `most_viewed` | `newest_first` |

### 2b. Apply Filters

Use jq on the discovery JSONL to filter:
```bash
cat gurukul/raw/channel-manifests/<channel-name>-discovery.jsonl | jq -c 'select(...filters...)' 
```

### 2c. Confirm Selection

Show filtered count and a preview (first 5 + last 5 titles). Confirm with user.

### 2d. Ask for Batch Emphasis

Ask: "What should the wiki emphasize for this channel? (or 'default' for balanced coverage)"

This replaces the per-video Step 4 (Discuss with User) from `/ingest`.

### 2e. Create Manifest

Create `gurukul/raw/channel-manifests/<channel-name>.json`:

```json
{
  "channel": {
    "name": "<channel-name>",
    "url": "<channel-url>",
    "fetched_at": "YYYY-MM-DDTHH:MM:SSZ",
    "total_videos": 150
  },
  "filters": {
    "dateafter": null,
    "datebefore": null,
    "title_contains": null,
    "title_excludes": null,
    "min_views": null,
    "max_count": null,
    "sort": "newest_first"
  },
  "batch_emphasis": "default",
  "videos": [
    {
      "id": "dQw4w9WgXcQ",
      "title": "Video Title",
      "url": "https://www.youtube.com/watch?v=dQw4w9WgXcQ",
      "upload_date": "20230615",
      "duration": 623,
      "view_count": 50000,
      "status": "pending",
      "error": null,
      "ingested_at": null,
      "raw_file": null,
      "wiki_pages": []
    }
  ]
}
```

Ensure `gurukul/raw/channel-manifests/` directory exists before writing:
```bash
mkdir -p gurukul/raw/channel-manifests
```

## Phase 3: Batch Ingestion

### 3a. Serial Caption Download

Download captions for all `pending` videos, one at a time with sleep intervals:

```bash
/opt/homebrew/bin/yt-dlp --write-subs --write-auto-subs --sub-lang "en" --sub-format vtt \
  --skip-download --no-overwrites \
  --sleep-subtitles 3 --sleep-requests 1 \
  -o "/tmp/channel-ingest/%(id)s" "<video_url>"
```

For each video:
1. Download VTT captions to `/tmp/channel-ingest/`
2. Parse VTT → clean text (strip headers, timestamps, duplicate lines, `[Music]` tags)
3. Save to `gurukul/raw/<kebab-case-title>.md` via obsidian-cli:
   ```bash
   obsidian vault=the_new_order create \
     path="gurukul/raw/<kebab-case-title>.md" \
     content="---\ntitle: \"<Video Title>\"\nsource_url: \"<video_url>\"\nspeaker: \"<channel_name>\"\ndate: YYYY-MM-DD\ntype: transcript\n---\n\n<cleaned transcript>" \
     silent
   ```
   Frontmatter shape:
   ```yaml
   ---
   title: "<Video Title>"
   source_url: "<video_url>"
   speaker: "<channel_name>"
   date: YYYY-MM-DD
   type: transcript
   ---
   ```
4. Update manifest: `status` → `downloaded`, set `raw_file`
5. If no English captions available → `status` → `skipped`
6. If video unavailable/private → `status` → `skipped`

**Rate limit handling**: If yt-dlp returns 429 or rate-limit error, increase sleep to 10s and retry up to 3 times. If still failing, mark as `failed` with error message.

Save the manifest to disk after each video's caption download (for resumability).

### 3b. Serial LLM Processing

Process each `downloaded` video one at a time. Spawn a fresh agent for each video.

**Agent prompt template**:
```
Process this YouTube video into the wiki.

Video: "<title>" (<url>)
Raw transcript: raw/<raw_file> (already downloaded)

Invoke the /ingest skill for this video. Modifications for batch mode:
- Step 1 (Acquire): SKIP — transcript is already saved in gurukul/raw/
- Step 4 (Discuss with User): SKIP — use this emphasis instead: "<batch_emphasis>"
- File I/O: all markdown writes to gurukul/ go through `obsidian vault=the_new_order ...`
  (see the File I/O section in /ingest). Fall back to Write silently if obsidian is unreachable.
- IGNORE all promotional content: sponsor segments, product plugs, affiliate pitches,
  merch pushes, course/membership upsells, Patreon/newsletter CTAs, discount codes,
  "check out my..." sections. Do NOT create wiki pages for the creator's own products
  or services. Do NOT include promotional claims in any wiki page. Extract only the
  substantive educational/informational content.

Execute Steps 2 (if frontmatter needs fixing), 3, 5, 5b (backlink to raw), 6, 7, 8 normally.

Report back: list of wiki page paths created/updated.
```

Dispatch each agent using:
```
Agent tool: subagent_type="general-purpose", mode="bypassPermissions"
```

**Coordinator flow** (this skill, between agents):
1. Read manifest, find next `downloaded` video
2. Dispatch agent with prompt above, wait for completion
3. Parse agent result for wiki page paths
4. Update manifest: `status` → `done`, set `ingested_at`, populate `wiki_pages`
5. If agent fails: `status` → `failed`, record error, continue to next video
6. Save manifest to disk
7. Display progress:
   ```
   Channel: @channelname
   Progress: 23/50 (46%) | Done: 23 | Pending: 25 | Skipped: 2
   
   Last completed:
     - "Video Title A" → 7 wiki pages
     - "Video Title B" → 4 wiki pages
   ```
8. Repeat until no `downloaded` videos remain

### 3c. Finalize

After all videos are processed:

1. Append a summary entry to `gurukul/wiki/log.md`:
   ```bash
   obsidian vault=the_new_order append \
     path="gurukul/wiki/log.md" \
     content="- **ingest-channel**: Completed @<channel> — X/Y videos ingested, Z skipped, W failed"
   ```
2. Clean up temp files:
   ```bash
   rm -rf /tmp/channel-ingest/
   ```
3. Show final summary to user with counts and any failures worth retrying.

## Error Handling

| Scenario | Action |
|----------|--------|
| No English captions | Status → `skipped` |
| yt-dlp rate limited (429) | Increase sleep to 10s, retry 3x, then `failed` |
| Video unavailable/private | Status → `skipped` |
| Agent fails on wiki writing | Status → `failed`, log error, continue to next |
| Session interrupted | Manifest persists on disk, resume next session |

## Resumability

The manifest is the single source of truth. On every invocation:
1. Check for existing manifest in `gurukul/raw/channel-manifests/`
2. If found, show status breakdown and offer to resume
3. Resume skips `done` and `skipped` videos, retries `failed` if user wants
4. Caption download resumes from first `pending` video
5. LLM processing resumes from first `downloaded` video

## Notes

- **Serial only** — no parallel agents. Channels cover overlapping topics, so parallel wiki writes would conflict.
- Each agent invokes `/ingest` via the Skill tool — all wiki conventions (frontmatter, index, log, wikilinks) are enforced by `/ingest`.
- The manifest is updated and saved after every single video operation for crash safety.
- Use `/opt/homebrew/bin/yt-dlp` (full path) for all yt-dlp commands.
