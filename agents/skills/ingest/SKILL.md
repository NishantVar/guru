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

## File I/O

All markdown reads and writes inside `gurukul/` (raw transcripts, wiki pages, `index.md`, `log.md`) go through the `obsidian` CLI against the `the_new_order` vault. This repo is registered as that vault, so wikilinks, backlinks, and live indexing work automatically.

- Create: `obsidian vault=the_new_order create path="<path-from-repo-root>" content="..." silent`
- Overwrite: add the `overwrite` flag
- Append: `obsidian vault=the_new_order append path="<path>" content="..."`
- Read: `obsidian vault=the_new_order read path="<path>"`
- Use `\n` in `content=` values for newlines.

Non-markdown artefacts (VTT subtitles, JSON manifests, `/tmp/` scratch files) use normal tools (`Write`, `Edit`, `Bash`). If `obsidian` is unreachable (vault not open), silently fall back to `Write`/`Edit` — do not surface this to the user.

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

Save the acquired content as `gurukul/raw/<kebab-case-title>.md` via obsidian-cli:

```bash
obsidian vault=the_new_order create \
  path="gurukul/raw/<kebab-case-title>.md" \
  content="---\ntitle: \"<Title>\"\nsource_url: \"<URL if applicable>\"\nspeaker: \"<Author/Channel/Speaker if applicable>\"\ndate: YYYY-MM-DD\ntype: article | transcript | paper | notes\n---\n\n<content here>" \
  silent
```

Frontmatter shape:
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
- The frontmatter and source content are immutable once created. The only permitted modification is the append-only "Wiki pages generated from this source" section at the bottom, managed by Step 5b.
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

Writes go through obsidian-cli:
```bash
# New page
obsidian vault=the_new_order create \
  path="gurukul/wiki/unconfirmed/<slug>.md" content="..." silent

# Update existing page (replaces contents)
obsidian vault=the_new_order create \
  path="gurukul/wiki/unconfirmed/<slug>.md" content="..." silent overwrite

# Check if a page exists before creating (read returns non-zero on miss)
obsidian vault=the_new_order read path="gurukul/wiki/unconfirmed/<slug>.md" >/dev/null 2>&1
```

## Step 5b: Backlink to Raw

After all wiki pages for this ingest are written, append a "Wiki pages generated from this source" section to the raw file so the raw → wiki direction is explicit (not just reliant on Obsidian's backlink index).

Structure:

```markdown

---

## Wiki pages generated from this source
<!-- Managed by /ingest — append-only. Do not hand-edit; original content above is immutable. -->

### Run YYYY-MM-DD
- [[wiki/unconfirmed/<page-slug>]] — <one-line description, <80 chars>
- [[wiki/unconfirmed/<page-slug>]] — <one-line description>
```

Every re-ingest of the same source adds a new `### Run YYYY-MM-DD` block under the same `## Wiki pages generated from this source` heading — never rewrites earlier runs, never dedupes. History stays auditable.

Implementation:

```bash
# Step 1: ensure the section header exists (idempotent — only appends on first run)
obsidian vault=the_new_order read path="gurukul/raw/<slug>.md" \
  | grep -q "^## Wiki pages generated from this source" \
  || obsidian vault=the_new_order append \
       path="gurukul/raw/<slug>.md" \
       content="\n---\n\n## Wiki pages generated from this source\n<!-- Managed by /ingest — append-only. Do not hand-edit; original content above is immutable. -->"

# Step 2: append this run's block
obsidian vault=the_new_order append \
  path="gurukul/raw/<slug>.md" \
  content="\n### Run $(date +%Y-%m-%d)\n- [[wiki/unconfirmed/<page-1>]] — <description>\n- [[wiki/unconfirmed/<page-2>]] — <description>"
```

One-line description convention: pull from the wiki page's first sentence under its `## Summary` (or opening paragraph), trimmed to <80 chars. Every page listed here must also list this raw file in its own `sources:` frontmatter — the two sides stay in sync.

## Step 6: Update Index

Add new pages to `gurukul/wiki/index.md` under the right topic/type:
```bash
obsidian vault=the_new_order append \
  path="gurukul/wiki/index.md" \
  content="- [[wiki/unconfirmed/<slug>]] — <one-line description>"
```
If a specific topic/type section needs to be edited in-place rather than appended, read the file, edit, and rewrite with `create ... overwrite`.

## Step 7: Handle Learning Content

SKIP — do NOT create or update anything in `gurukul/lessons/`. All content goes into `gurukul/wiki/unconfirmed/` only. The lessons folder is maintained separately and is off-limits during ingestion.

## Step 8: Log It

Append an entry to `gurukul/wiki/log.md`:
```bash
obsidian vault=the_new_order append \
  path="gurukul/wiki/log.md" \
  content="- **ingest**: Processed [[raw/<slug>]] → created [[wiki/unconfirmed/<page1>]], updated [[wiki/unconfirmed/<page2>]]"
```

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
- Never modify the frontmatter or source content of files in `gurukul/raw/` after creation. The only permitted change is appending to the "Wiki pages generated from this source" section at the bottom (Step 5b).
