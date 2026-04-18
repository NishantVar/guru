# The New Order — LLM Wiki Schema

This is an LLM-maintained wiki inside an Obsidian vault. Claude Code is the maintainer. The wiki compounds knowledge over time through incremental ingestion, not re-derivation.

## Architecture

```
the_new_order/
├── gurukul/                  # Knowledge base and curriculum (separate git repo)
│   ├── raw/                  # Immutable source documents (articles, papers, transcripts, notes)
│   │   ├── assets/           # Downloaded images, PDFs, media
│   │   └── channel-manifests/  # Per-channel ingestion state (discovery JSONL + manifest JSON)
│   ├── wiki/                 # LLM-generated knowledge pages
│   │   ├── confirmed/        # Human-reviewed and validated pages
│   │   ├── unconfirmed/      # Auto-generated pages not yet reviewed
│   │   ├── index.md          # Master catalog of all wiki content
│   │   ├── log.md            # Chronological operation log
│   │   └── AGENTS.md         # Wiki-specific agent instructions
│   ├── lessons/              # Curriculum for external learners
│   │   ├── index.md          # Learning path catalog & curriculum overview
│   │   └── <track>/          # One subdirectory per track (discover dynamically)
│   ├── docs/                 # Curriculum design docs (ladder, track definitions)
│   └── AGENTS.md             # Sensei-specific agent instructions
├── agents/                   # Visible folder for Obsidian (skills, config)
│   └── skills/               # Local Claude Code skills
├── .agents -> agents
├── .claude -> agents
├── AGENTS.md                 # This file — schema & conventions
└── CLAUDE.md -> AGENTS.md
```

## Ownership

| Layer                      | Owner      | Rules                                                            |
| -------------------------- | ---------- | ---------------------------------------------------------------- |
| `gurukul/raw/`             | Human      | Immutable once placed. Never edit raw sources.                   |
| `gurukul/wiki/unconfirmed/`| LLM        | Claude creates auto-generated pages here.                        |
| `gurukul/wiki/confirmed/`  | Co-evolved | Human reviews and moves pages here. Claude maintains content.    |
| `gurukul/lessons/`         | LLM        | Claude creates and maintains lessons. Human reviews for quality. |
| `AGENTS.md`                | Co-evolved | Either party can propose changes.                                |

## Confirmed vs Unconfirmed

All newly ingested wiki pages land in `gurukul/wiki/unconfirmed/`. A page moves to `gurukul/wiki/confirmed/` after a human reviews and validates it. Both folders share the same wiki conventions (frontmatter, wikilinks, page types).

- **Unconfirmed**: auto-generated, may contain inaccuracies, not yet human-reviewed
- **Confirmed**: human-reviewed and validated as accurate

When a page moves from unconfirmed → confirmed, update all wikilinks that reference it (change `[[wiki/unconfirmed/X]]` to `[[wiki/confirmed/X]]`).

## Three Operations

### 1. Ingest

Trigger: User provides a URL, file, text, or drops something in `gurukul/raw/`.

Use the `/ingest` skill. It handles all source types (articles, videos, podcasts, local files, text) including transcription, knowledge extraction, wiki page creation, indexing, and logging. New pages always go to `gurukul/wiki/unconfirmed/`.

For entire YouTube channels, use `/ingest-channel`. It discovers all videos, lets the user filter, batch-downloads captions, and processes each video through `/ingest` serially with progress tracking and resumability. State is persisted in `gurukul/raw/channel-manifests/`.

### 2. Query

Trigger: User asks a question.

1. **Read `gurukul/wiki/index.md`** to find relevant pages.
2. **Read relevant wiki pages** (from both confirmed/ and unconfirmed/) and synthesize an answer.
3. **Cite sources** using `[[wikilinks]]` to wiki pages and `[[raw/source]]` files.
4. **File reusable answers.** If the answer is substantial, create a new wiki page (type: synthesis) in `gurukul/wiki/unconfirmed/` and add it to the index.
5. **Log it** if a new page was created.

### 3. Lint

Trigger: User asks to lint, health-check, or maintain the wiki.

Report findings to the user, then fix issues with their approval. See `gurukul/wiki/AGENTS.md` and `gurukul/AGENTS.md` for domain-specific lint checks.

## File Naming

- Wiki pages: `gurukul/wiki/unconfirmed/<kebab-case-topic>.md` (or `confirmed/` after review)
- Raw sources: `gurukul/raw/<kebab-case-title>.md`
- Lessons: `gurukul/lessons/<track>/<kebab-case-lesson>.md`

## Log Format (gurukul/wiki/log.md)
```markdown
## YYYY-MM-DD

- **ingest**: Processed [[raw/filename]] → created [[wiki/unconfirmed/page1]], updated [[wiki/unconfirmed/page2]]
- **query**: "Question asked" → created [[wiki/unconfirmed/page3]]
- **lint**: Fixed 2 orphan pages, resolved 1 contradiction
- **confirm**: Moved [[wiki/confirmed/page4]] from unconfirmed after review
```

## Common Mistakes

| Mistake | Fix |
|---------|-----|
| Editing raw sources | Never. Raw is immutable. |
| Forgetting to log operations | Always append to `gurukul/wiki/log.md` as the last step |
| Missing wikilinks | Link aggressively between related pages |
| Creating standalone CLAUDE.md | Always create AGENTS.md and symlink CLAUDE.md to it |
| Putting new pages in confirmed/ | New ingested pages always go to unconfirmed/ |
