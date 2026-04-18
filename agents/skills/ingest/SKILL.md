---
name: ingest
description: Ingest any source into the wiki — URLs (articles, videos, podcasts), local files, or raw text. Handles transcription, knowledge extraction, wiki page creation, and indexing.
---

# Ingest Source

Takes any source material and turns it into wiki knowledge.

## Input

The user provides one or more of:
- A URL (article, blog post, YouTube video, podcast, any web page)
- A local file path (audio, video, PDF, text)
- Raw text / pasted content
- A file already in `gurukul/raw/`

## Step 1: Acquire Source

Determine source type and get the content into `gurukul/raw/`.

### Web Articles / Blog Posts / Web Pages

```bash
# Fetch and save as markdown
```
Use WebFetch to retrieve the page, clean to markdown, save to `gurukul/raw/<kebab-case-title>.md`.

### YouTube URLs

Use `yt-dlp` to grab existing captions (fastest, no AI needed):

```bash
# Get metadata
yt-dlp --print title --print channel --print upload_date --print duration_string --no-download "<URL>"

# Get transcript — prefer manual subs, fall back to auto-generated
yt-dlp --write-subs --write-auto-subs --sub-lang "en" --sub-format vtt --skip-download --no-overwrites -o "/tmp/yt-transcript" "<URL>"
```

Parse the `.vtt` file: strip VTT headers, timestamps, and duplicate lines to produce clean text.

If no English subtitles are available, fall back to downloading audio and using Whisper (see below).

### Other Video/Audio URLs (Vimeo, Twitter/X, Podcasts, etc.)

Download audio only, then transcribe:

```bash
# Download audio
yt-dlp -x --audio-format mp3 -o "/tmp/%(title)s.%(ext)s" "<URL>"

# Transcribe with Whisper (base model by default)
whisper "/tmp/<filename>.mp3" --model base --output_format txt --output_dir /tmp/
```

### Local Audio/Video Files

Transcribe directly:

```bash
whisper "<filepath>" --model base --output_format txt --output_dir /tmp/
```

For large files (>1 hour), consider `--model tiny` for speed or `--model small` for better accuracy.

### Raw Text / Pasted Content

Save directly to `gurukul/raw/<kebab-case-title>.md`.

### File Already in `gurukul/raw/`

Read it directly, skip to Step 2.

## Step 2: Save to gurukul/raw/

Save the acquired content as `gurukul/raw/<kebab-case-title>.md` with frontmatter:

```markdown
---
title: "<Title>"
source_url: "<URL if applicable>"
speaker: "<Author/Channel/Speaker if applicable>"
date: YYYY-MM-DD
type: article | transcript | paper | notes
---

<content here>
```

Rules:
- Kebab-case the title for the filename
- This file is immutable once created — never edit it later
- For transcripts: clean up artifacts (repeated lines, timing codes, `[Music]` tags)

## Step 3: Extract Knowledge

Read the source and extract:
- a. Entities, concepts, claims, techniques, data points
- b. Notable exact quotes with speaker/author attribution
- c. All references mentioned (books, tools, projects, people, URLs)

## Step 4: Discuss with User

- Distill a one-sentence takeaway first
- Summarize key takeaways
- Ask what to emphasize, what's most relevant

## Step 5: Write Wiki Pages

- For each significant entity/concept, create or update a page in `gurukul/wiki/unconfirmed/`
- A single source may touch 5-15 wiki pages — don't be conservative
- For each reference cataloged in step 3c, check if a wiki page exists; if not, create a stub entity page
- Every wiki page needs YAML frontmatter (title, created, updated, sources, tags, type)
- Use `[[wikilinks]]` liberally for cross-references
- Never delete information — use ~~strikethrough~~ for outdated claims

## Step 6: Update Index

Add new pages to `gurukul/wiki/index.md` under the right topic/type.

## Step 7: Handle Learning Content

SKIP — do NOT create or update anything in `gurukul/lessons/`. All content goes into `gurukul/wiki/unconfirmed/` only. The lessons folder is maintained separately and is off-limits during ingestion.

## Step 8: Log It

Append an entry to `gurukul/wiki/log.md`.

## Cleanup

After successful ingest, remove temporary files from `/tmp/`:
```bash
rm -f /tmp/yt-transcript.* /tmp/*.mp3 /tmp/*.txt
```

## Notes

- YouTube captions are preferred over Whisper — they're instant and free
- Whisper `base` model is the default (good speed/quality balance on Apple Silicon)
- If the user says "use small" or "use medium", pass that model to Whisper instead
- For batch processing (multiple URLs), process them one at a time sequentially
- Never modify files in `gurukul/raw/` after creation
