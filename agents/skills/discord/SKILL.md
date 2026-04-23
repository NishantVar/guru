---
name: discord
description: Manage "The New Order" AI learning Discord server. Use whenever the user mentions Discord, publishing lessons, updating the index, managing forums, syncing curriculum to Discord, or posting content. Also triggers on - forum post, index channel, pinned message, lesson publish, curriculum roadmap, track forum, Discord channel structure. Even if the user just says "post this lesson" or "update the index" without mentioning Discord, use this skill if the context is about their AI teaching project.
---

# The New Order — AI Learning Discord Server

You manage the **The New Order** Discord server (Guild ID: `1491325916297822318`) where lessons from the `gurukul/` curriculum are published for learners. This skill handles **distribution** — posting lessons to forums, keeping the index channel in sync, and managing server structure. **Lesson authoring lives in `/write_lesson`.**

Use the Discord MCP tools for all server actions. If the MCP is not connected, tell the user to restart the session.

## Source of Truth

- **Local lessons**: `gurukul/lessons/<track>/<slug>.md` — source of truth
- **Local index**: `gurukul/lessons/index.md` — source of truth for the Discord pinned roadmap
- **Discord forums**: distribution channel only — always mirror the local files
- **Channel/forum IDs**: fetched on demand via `list_channels` — no need to track locally

See `$OBSIDIAN/the_new_order/CLAUDE.md` and `gurukul/AGENTS.md` for lesson conventions and frontmatter schema.

## Server Structure

The curriculum follows the **Agentic Engineering Ladder** (see `gurukul/docs/agentic-engineering-ladder.md`). Tracks are not hard-coded here — discover them by listing subdirectories of `gurukul/lessons/`. Each subdirectory is a track and maps to one forum channel.

### Channels

Under the **Learning** category:

| Channel | Type | Purpose |
|---------|------|---------|
| `index` | text | Pinned roadmap message — the entry point |
| `briefings` | forum | Orientation, curriculum structure, announcements (maps to `gurukul/lessons/briefings/`) |
| One forum per ladder track | forum | `vibing`, `operating`, `directing`, `delegating`, `orchestrating`, `systemizing`, `compounding`, `evolving` — lessons for that level |

Discover the current list of tracks dynamically. Only create forums for tracks that have (or will have) content — it's fine to grow the server as the curriculum grows.

### Forum Tags

Tags follow the `lesson_format` schema from the sensei frontmatter, plus a few cross-cutting tags:

- **Per-track forums**: `concept`, `lab`, `project`, `drill`, `case-study`, `discussion`
- **briefings**: `welcome`, `curriculum`, `announcement`, `update`

Apply the tag matching the lesson's `lesson_format` when posting. Briefings use a tag matching their topic.

### Channel Descriptions

- `index`: "Start here — your learning roadmap"
- `briefings`: "Curriculum orientation and announcements"
- Track forums: use the track's one-line description from `gurukul/docs/agentic-engineering-ladder.md`

## Index Channel

The `index` channel holds a single **pinned message** — the master roadmap. It mirrors `gurukul/lessons/index.md`.

**Critical rule**: Never create a new pinned message. Always **edit the existing one**. Find it via `list_pinned_messages` in the `index` channel. If the index message doesn't exist yet, create it once and pin it.

### Format

Mirror the structure of `gurukul/lessons/index.md` but render for Discord (no wikilinks — use forum post links or bare titles):

```
**The New Order — Learning Roadmap**

Start with Briefings, then pick a track and follow it in order.

**Briefings**
1. Welcome — the curriculum's premise
2. The Curriculum — how the 8 tracks work

**Vibing** — use a coding agent naturally
1. Setting Up Coding Agents — install Claude Code and get to first output
2. Just Ask — plain-language prompts for real work
…

**Operating** — tools and superpowers that amplify the agent
1. Voice to Text — dictate everywhere
…

---
*Source: gurukul/lessons/index.md. Lessons are added as the curriculum grows.*
```

Link each lesson title to its forum post URL when the post exists.

## Lesson Posts

Each lesson in `gurukul/lessons/<track>/<slug>.md` maps to one forum post in the matching track forum. Briefings go to the `briefings` forum.

**Post title**: the lesson's `title` from frontmatter.

**Post body**: the lesson's markdown content with frontmatter **stripped**. Keep everything else verbatim — the lesson is already written for Discord consumption (short sections, scannable blocks, front-loaded sentences per `/write_lesson` conventions).

**Tables**: Discord does not render markdown tables — pipe syntax displays as raw text. Convert all tables to **code blocks** with manually aligned columns:

```
\```
#  Track           What you'll learn
─  ──────────────  ──────────────────────────────────────────────
1  Vibing          Use a coding agent for real work
2  Operating       Set up tools that make your agent more capable
\```
```

For simple two-column tables where alignment isn't critical, a **bold + line break** list is also acceptable:

```
**Vibing** — Use a coding agent for real work
**Operating** — Set up tools that make your agent more capable
```

Prefer code blocks for data-heavy or multi-column tables. Prefer bold lists for simple key-value pairs.

**Chunking for long lessons**: Discord limits messages to 2000 characters. Most lessons exceed this. Split the body into chunks under 2000 chars each, breaking at natural section boundaries (before `##` or `###` headings). Post the first chunk as the forum post's initial content via `create_forum_post`. Send each subsequent chunk as a follow-up message in the same thread using `send_message` with the thread ID (returned from `create_forum_post`) as the channel. The next-lesson link goes in the final chunk.

**Tag**: the lesson's `lesson_format` (or an appropriate briefings tag).

**Cross-lesson links**: if the lesson body contains `[[gurukul/lessons/<track>/<slug>]]` wikilinks, replace them with Discord forum post URLs before posting. If the target post doesn't exist yet, replace with the plain title and flag it to the user.

**Next-lesson link**: Always append a "next lesson" link at the bottom of every post. Use `gurukul/lessons/index.md` to determine lesson order within the track. If the next lesson's forum post exists, link to it; otherwise use the plain title. Format:

```
---
**Next →** [Lesson Title](forum-post-url)
```

If this is the last lesson in the track and the next track in the index has lessons, link to the first lesson of that next track with a note:

```
---
**Next →** [Lesson Title](forum-post-url) *(in the Operating track)*
```

If this is the very last lesson across all tracks, omit the next link.

When publishing a batch of lessons, do a second pass to backfill next-links for any posts that were created before their target existed.

## Workflows

### Publishing a New Lesson

Precondition: the lesson file already exists at `gurukul/lessons/<track>/<slug>.md` (produced by `/write_lesson`).

1. Read the lesson file and extract: `title`, `track`, `lesson_format`, body
2. Confirm the matching forum channel exists; create it if not (see "Setting Up or Expanding the Server")
3. Strip frontmatter and resolve any `[[wikilinks]]` to Discord URLs or plain titles
4. Create the forum post with the title, body, and `lesson_format` tag
5. Append the next-lesson link (see "Next-lesson link" above)
6. Update the index channel's pinned message to include the new lesson (read `gurukul/lessons/index.md` as source of truth, re-render for Discord, edit the pinned message)
7. If the previous lesson in the track exists as a forum post, edit it to add/update its next-lesson link pointing to this new post

### Updating an Existing Lesson

1. Read the updated lesson file
2. Find the corresponding forum post (search the track forum by title)
3. Edit the post body to match — strip frontmatter, resolve wikilinks
4. If the lesson's `updated:` date reflects a meaningful change, add a brief note at the top of the post body (e.g., "*Updated YYYY-MM-DD — expanded examples.*")
5. Re-sync the pinned index if the title or order changed

### Updating the Index

1. Read `gurukul/lessons/index.md`
2. Render it for Discord (convert wikilinks to forum URLs, apply the format above)
3. Edit the pinned message in the `index` channel

Never post a new index message. Always edit the existing one.

### Setting Up or Expanding the Server

When the server is missing structure (as it currently is — only the `index` channel exists under the Learning category):

1. Ensure a **Learning** category exists
2. For each subdirectory in `gurukul/lessons/` that has or will have content, create a forum channel under Learning:
   - `briefings` forum
   - One forum per ladder track (create lazily — only for tracks that have lessons, or that you're about to publish into)
3. Add the tags listed in "Forum Tags" to each forum
4. Set each forum's description
5. If the pinned index message doesn't yet match `gurukul/lessons/index.md`, edit it to mirror the local index
Grow the server incrementally — don't pre-create empty forums for tracks that have no lessons yet.

### Archiving / Deprecating

- **Outdated lesson**: prefer editing the post with a "Outdated — see [new lesson]" header over deleting. Forum posts may have useful discussion.
- **Removed lesson**: remove from the pinned index; leave the forum post in place with a note.
- **Removed track**: remove the forum from the pinned index; leave the forum channel itself unless the user explicitly wants it deleted.

## Handling Current Server State

As of the last recorded state, the server has **no track forums** — only the `index` channel in the Learning category holds a placeholder pinned message. When the user wants to publish a lesson:

1. Create the needed forum(s) for the lesson's track (and `briefings` if publishing a briefing)
2. Replace the placeholder pinned index with the rendered `gurukul/lessons/index.md`
3. Proceed with the publish workflow

## References

- `gurukul/docs/agentic-engineering-ladder.md` — canonical track progression and descriptions
- `gurukul/AGENTS.md` — lesson frontmatter and naming conventions
- `agents/skills/write_lesson/SKILL.md` — owns lesson authoring; don't duplicate format guidance here
