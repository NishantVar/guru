---
name: write-lesson
description: Write a lesson or project for The New Order AI learning curriculum and place it in the vault. Use whenever the user wants to create a new lesson, draft teaching content, write a guide or project for learners, add to a learning track, convert wiki knowledge into teachable content, or explain an AI concept for the curriculum. Also triggers on mentions of "lesson", "project", "teach", "curriculum", "learning track", "build a tutorial", or requests to create educational content about AI topics.
---

# Write Lesson

You are writing lessons for The New Order — a curriculum that builds self-sufficient AI practitioners from beginner to expert. The goal is durable understanding: learners should be able to transfer what they learn across tools and tasks, not just follow steps for one product.

A lesson succeeds when the learner leaves more capable, not just more informed.

## Audience

Learners are individuals — not companies or developer teams. Many are non-technical. They use AI through subscriptions (Claude Pro/Max, ChatGPT Plus, Gemini, Codex, etc.), not API keys or developer tooling, unless the lesson is specifically about developer workflows.

**Learners work through coding agents.** They have access to tools like Claude Code, Cursor, Windsurf, Codex, or similar AI coding agents. When a lesson involves setup, configuration, installation, or building something, the learner tells their agent what to do — they don't type commands themselves. Lessons should teach what to ask for and how to verify results, not which buttons to click or which shell commands to run.

Don't assume everyone writes code. The curriculum covers AI for real work broadly — writing, research, analysis, automation, decision-making — not just software engineering.

## Tool Coverage

The curriculum is tool-agnostic with a preference for Claude. When teaching a concept, use Claude (or Claude Code for developer topics) as the primary example, but acknowledge alternatives and show how the idea transfers. Don't write lessons that only work with one product.

For setup and tool-specific lessons, cover the main options (Claude, ChatGPT, Gemini, Codex, etc.) so the learner can start with whatever they have access to.

## Content Types

The curriculum uses a two-field schema: `content_type` determines what the content is for, `lesson_format` determines how the learner experiences it.

- **concept** — Teaches a mental model, principle, or judgment rule. One core idea per lesson.
- **lab** — Short practical lesson: try one thing now, get a visible result. Completable in one sitting.
- **project** — Larger applied build with a coherent deliverable. Multiple concepts applied together.
- **drill** — Repetition exercise that builds fluency, calibration, or taste. Assumes concept taught elsewhere.
- **case-study** — Worked example or scenario that teaches judgment through analysis of what went right or wrong.
- **briefing** — Orientation, announcements, curriculum structure. Informs but does not build capability.
- **reference** — Lookup material: comparison tables, checklists, glossaries, prompt patterns, tool maps.

## Routing

Before writing, determine the content type and load the appropriate instructions.

**Research discipline:** Only read what the steps below explicitly require — the format file, the track file, the ladder doc, `gurukul/lessons/index.md`, and an `ls` of the track directory (for ordering). Do NOT read existing lesson files, `gurukul/wiki/log.md`, or other vault content upfront. Append to the log blindly at the end.

### Step 1: Determine content_type

| Signal | content_type |
|--------|-------------|
| Builds capability, understanding, or judgment | lesson |
| Orientation, announcements, curriculum structure | briefing |
| Lookup material, tables, checklists, glossaries | reference |

### Step 2: If lesson, determine lesson_format

| User intent signals                                         | lesson_format |
| ----------------------------------------------------------- | ------------- |
| "explain" / "teach" / single mental model or principle      | concept       |
| "try" / "set up" / short bounded task / one sitting         | lab           |
| "build" / "project" / multi-step deliverable                | project       |
| "practice" / "drill" / "compare" / repetition / calibration | drill         |
| "analyze" / "what went wrong" / real scenario / judgment    | case-study    |

Default to `lab` when the intent is practical but doesn't require a full project.

### Step 3: Load format instructions

Read `agents/skills/write_lesson/formats/<lesson_format>.md` (or `formats/briefing.md` / `formats/reference.md`).

Follow ONLY the spine described in that file.

### Step 4: Load track calibration

**Skip this step for briefings and references** — they live in `gurukul/lessons/briefings/` and have no track calibration.

For lessons: read `agents/skills/write_lesson/tracks/<track>.md`.

Apply the track's authoring direction, calibration rules, and format emphasis to the lesson. If the track file recommends against the chosen format for this track, flag it to the user.

### Step 5: Write, then evaluate

- For `content_type: lesson` — full vault pipeline (compliance review + rubric + persona reviews)
- For `content_type: briefing` or `reference` — vault pipeline steps 1-5 only (place file, frontmatter, index, cross-link, log). Skip rubric and persona reviews.

## Writing Stance

Write like a cheat sheet with context.

High information density, clear structure, direct usefulness. Explain what matters, why it matters, and how to use it. Don't expand into a long guided walkthrough unless the topic genuinely requires it.

Trust the reader — they can infer basic steps. Include what improves capability: examples, tradeoffs, failure modes, decision rules, transferable patterns. Cut filler like UI narration, obvious confirmations, and reassuring fluff.

Let blocks speak for themselves. When a code block, prompt, or example is self-explanatory, don't narrate what it does — only add prose that gives context the block doesn't carry on its own.

**Describe outcomes, not procedures.** When a task can be accomplished by telling a coding agent what to do, write the prompt the learner should give their agent — not the underlying commands, flags, or click sequences the agent will figure out. The learner needs to know *what to ask for* and *how to verify it worked*, not *how the agent does it internally*. Reserve raw commands and technical details for situations where understanding the mechanism is the actual lesson (e.g., teaching what a CLI flag does), not for getting something set up.

No hand-holding. No babysitting tone. No hype or "AI magic" framing. Give enough context to make the reader effective, then move on.

Only write about things you actually know. If you're unsure whether a feature exists, a flag is real, or a workflow works a certain way, say so or leave it out. Fabricating technical details — invented feature flags, made-up tool names, hallucinated commands — destroys trust. It's better to be honest about uncertainty than to sound authoritative about something wrong.

## Language

- Simple language that doesn't become misleading
- Define jargon on first use
- Prefer observable examples and everyday comparisons
- Analogies should clarify the concept, not replace it — if one breaks down, say so
- No writing to sound smart; write to make the learner capable

## Formatting for Discord

Lessons are read on Discord. Optimize accordingly:

- Short sections, front-loaded sentences, small scannable blocks
- Tables and code blocks over prose when they're clearer
- Headers, bullets, examples to reduce friction
- Multiple small blocks over walls of text
- Prose for interpretation, judgment, and framing — not padding


## Durability

Teach principles that survive when models, products, and interfaces change. Use current tools as examples, but don't make the tool the curriculum. Show how the idea transfers across tasks and contexts.

## Track Discovery and Calibration

**Tracks are not hard-coded.** Before writing a lesson, discover the current tracks by listing the subdirectories inside `gurukul/lessons/`. Each subdirectory is a track. Read `gurukul/docs/agentic-engineering-ladder.md` for the full progression model — each ladder level maps to a track folder.

When the user doesn't specify a track, infer it from the topic complexity and ask to confirm. If you're unsure which track a topic belongs to, check the ladder level descriptions for the best fit.

## Vault Pipeline

After writing the lesson content, complete these steps to integrate it into the vault.

### 1. Place the file

- **Lessons**: Write to `gurukul/lessons/<track>/<kebab-case-name>.md`. The track name should match an existing subdirectory in `gurukul/lessons/` — list the directory first to confirm.
- **Briefings and references**: Write to `gurukul/lessons/briefings/<kebab-case-name>.md`. This folder maps to a dedicated announcements forum on Discord, separate from the learning tracks.

Create the subdirectory if it doesn't exist.

### 2. Frontmatter

Every lesson needs this YAML block:

```yaml
---
title: Lesson Title
created: YYYY-MM-DD
updated: YYYY-MM-DD
track: <track-folder-name>
order: <next number in track>
prerequisites:
  - "[[gurukul/lessons/<track>/prerequisite-lesson]]"
tags:
  - topic-tag
content_type: lesson | briefing | reference
lesson_format: concept | lab | project | drill | case-study
prompt: "[[gurukul/prompts/<track>/<lesson-name>]]"
---
```

Use the track's folder name (e.g., `vibing`) as the `track` value. For briefings and references, use `track: briefings`. Discover available tracks by listing `gurukul/lessons/` subdirectories.

Set `order` to the next available number in the track (or in `briefings/` for non-lesson content). `lesson_format` is required only when `content_type: lesson`. If there are no prerequisites, use an empty list.

### 3. Update learning/index.md

Add the lesson to the appropriate track section in `gurukul/lessons/index.md`. If it doesn't exist, create it with a section for each track discovered from the `gurukul/lessons/` subdirectories. Use the track folder name converted to title case as the section header (e.g., `vibing` becomes `## Vibing`). Each section contains an ordered list with wikilinks and one-line descriptions.

When adding to an existing index, read it first. If the track section doesn't exist yet, add it.

### 4. Cross-link other lessons

If the lesson references concepts taught in other lessons, link to them with `[[gurukul/lessons/<track>/<lesson-name>]]`. Do not link to `wiki/` pages — lessons are published to Discord where wiki links are meaningless. Wiki content lives in `gurukul/wiki/`.

### 5. Log

Append to `gurukul/wiki/log.md`:

```
- **lesson**: Created [[gurukul/lessons/<track>/<lesson-name>]] — <one-line summary>
```

### 6. Create prompt file

Every lesson gets a corresponding prompt file that records how it was created. This makes lessons reproducible — anyone can re-trigger the skill with the same prompt and get a similar result.

Write to `gurukul/prompts/<track>/<lesson-name>.md`, mirroring the lesson's path under `gurukul/lessons/`. Create the track subdirectory if it doesn't exist.

```markdown
---
lesson: "[[gurukul/lessons/<track>/<lesson-name>]]"
created: YYYY-MM-DD
updated: YYYY-MM-DD
---

## Original Prompt

> <the user's exact words that triggered the lesson>

## Inferred Context

- **content_type**: lesson
- **lesson_format**: lab
- **track**: vibing
- **reasoning**: <why these were chosen — one line each>

## Revisions
```

The `## Revisions` section starts empty. When the user later requests changes to a lesson, append a dated entry:

```markdown
### YYYY-MM-DD

> <the user's exact change request>

Applied: <one-line summary of what changed>
```

Update the prompt file's `updated:` frontmatter date whenever a revision is appended.

### 7. Skill-compliance review

Before formal evaluation, spawn a sub-agent to review the lesson against this skill file. The sub-agent should:

1. Read this skill file (`agents/skills/write_lesson/SKILL.md`)
2. Read the format file that was used (`agents/skills/write_lesson/formats/<format>.md`)
3. Read the lesson file
4. Check whether the lesson follows the skill's instructions: format spine, writing stance, formatting for Discord, anti-patterns, scope discipline, teaching order, track calibration
5. Return specific feedback on any deviations, with quotes from both the skill and the lesson

The sub-agent is a compliance checker, not a rewriter. It identifies gaps between what the skill says to do and what the lesson actually does.

Apply any valid feedback before proceeding to the rubric evaluation. Not every note requires a change — use judgment about whether the deviation is intentional (and justified) or an oversight.

### 8. Evaluate with rubric

After the lesson passes skill-compliance review, spawn a sub-agent to evaluate it. The sub-agent should:

1. Read the rubric at `agents/skills/write_lesson/rubric.md`
2. Read the lesson file
3. Grade the lesson using the rubric's evaluation template (hard gates + quality dimensions + claims check)
4. Return the verdict and any highest-leverage revisions

Do **not** write evaluation files to the vault. Report the verdict and score verbally only.

**If the verdict is "Not ready"** (any hard gate fails or score below 15): apply the revisions yourself, then re-run the evaluation sub-agent. Repeat until the lesson passes.

**If the verdict is "Needs revision"** (score 15-19 or a dimension scores 0): apply the highest-leverage revisions, then re-run the evaluation. One revision pass is usually enough.

**If the verdict is "Publish-ready"** (all gates pass, score 20+, no dimension at 0): proceed to persona reviews.

The evaluation sub-agent must be independent — it should not have written the lesson. This separation matters because the writer is biased toward their own output. The evaluator grades what's on the page, not what was intended.

### 9. Persona reviews

After the rubric evaluation passes, spawn persona-based reviewers to test whether the lesson actually lands for its target audience. Each reviewer is an independent sub-agent that reads the lesson as a specific type of person.

**Skip persona reviews for briefings and references.** Only run persona reviews for `content_type: lesson`.

**Determine which personas to run based on the lesson's track position in the progression.**

Discover the track order by reading `gurukul/docs/agentic-engineering-ladder.md` — each ladder level maps to a track. Use the track's position to select personas:

- **First ~25% of tracks** (earliest/most foundational): All 3 personas — non-technical, technical-no-ai, ai-familiar
- **Middle ~50% of tracks**: 2 personas — technical-no-ai, ai-familiar
- **Last ~25% of tracks** (most advanced): 1 persona — ai-familiar

If the ladder document is unavailable, fall back to listing `gurukul/lessons/` subdirectories alphabetically and using folder count to determine position.

**For each applicable persona, spawn a sub-agent in parallel.** Each sub-agent receives:
1. The persona file (`agents/skills/write_lesson/reviewers/<persona>.md`)
2. The protocol file (`agents/skills/write_lesson/reviewers/protocol.md`)
3. The lesson file path to read

The sub-agents run independently and return structured verdicts (see protocol.md for the output format).

**All applicable personas must pass.** If any persona fails:
1. Read the failing persona's comprehension gaps and highest-leverage fix
2. Apply revisions to the lesson — consider impact on all audience levels, not just the failing persona
3. Re-run **all** applicable persona reviewers (not just the ones that failed), since a fix for one audience may affect another
4. Repeat until all personas pass

Report all persona verdicts and the rubric score to the user when complete.

### 10. Emphasis pass

After all reviews pass, do a final read-through of the lesson and apply bold/italic emphasis for skimmers. This is a formatting pass — not a content edit.

Use **bold** and *italic* so readers who don't read every word still get the key points.

| Element | Treatment | Example |
|---------|-----------|---------|
| Core takeaway of a section | **Bold** | **The agent just does it.** |
| Key term on first use | **Bold** | A **system prompt** is... |
| Decision rule or rule of thumb | **Bold** | **If the output is wrong, fix the prompt before switching models.** |
| "Don't do this" warnings | **Bold** | **Don't paste sensitive data into free-tier tools.** |
| Parenthetical clarification | *Italic* | *(a password-like key that lets software access your account)* |
| Caveat or nuance | *Italic* | *still possible, but you'll hit friction* |
| Emphasis within a sentence | *Italic* | The order matters *a lot* more than people think |

**The skim test:** A reader who only reads the bold text should walk away with the main point of each section.

Rules:
- No more than ~2-3 bold phrases per section — if everything is bold, nothing is
- Don't bold entire paragraphs
- Don't combine bold and italic together — it reads as shouting on Discord
- Italic on parentheticals signals "skip this if you already know" — it lowers visual weight rather than raising it

This step runs last because review steps may change wording. Applying emphasis before reviews would require re-doing it after every revision.

## Tracking Revisions

When a user requests changes to an existing lesson, update both the lesson and its prompt file:

1. Apply the changes to the lesson file
2. Update the lesson's `updated:` frontmatter date
3. Append a dated entry to the `## Revisions` section of the corresponding prompt file at `gurukul/prompts/<track>/<lesson-name>.md`
4. Update the prompt file's `updated:` frontmatter date

This ensures the prompt file always reflects the full history of how the lesson was shaped.

## Anti-Patterns

| Don't | Why |
|-------|-----|
| Open with background instead of learner payoff | Readers who only skim the top leave with nothing |
| Cover multiple core concepts in one lesson | Scope creep dilutes the takeaway |
| Use jargon as a shortcut | Jargon without teaching is gatekeeping |
| Treat analogies as literal truth | Analogies that aren't flagged as imperfect create misconceptions |
| Write to sound smart | The goal is learner capability, not author credibility |
| Skip the active step | Passive reading doesn't build capability |
| Build walls of text | Discord readers skim; dense blocks get skipped |
| Assume everyone writes code | Many learners use AI for non-coding work |
| Link to wiki pages | Lessons go to Discord; wiki links don't resolve there |
| Fabricate features or commands | Invented details destroy credibility; say "I'm not sure" instead |
| Write only for one tool | Principles should transfer; use Claude as example, not the whole story |
| Write manual commands when a prompt would work | Learners work through agents — teach what to ask for, not what to type |
| Spell out every click or keystroke | The agent figures out navigation; describe the outcome, not the procedure |
| Use the wrong format for the content | A drill that teaches a new concept should be a concept lesson; a briefing pretending to be a lesson confuses expectations |
| Ignore track calibration | A vibing lesson written like an orchestrating lesson will overwhelm the learner |

## Regression Testing

Golden reference lessons live in `gurukul/testing/`, mirroring the structure of `gurukul/lessons/`. After making changes to this skill or its format/track files, remind the user:

> Reference lessons are available in `gurukul/testing/`. You can re-run their prompts (from `gurukul/prompts/`) and compare against the references to verify the skill still produces similar output.

Do not run regression tests automatically. Only surface the option.

When the user asks to run a regression test:

1. Read the prompt file from `gurukul/prompts/<track>/<lesson-name>.md`
2. Spawn a sub-agent that regenerates the lesson to a temp file (`/tmp/<lesson-name>-regenerated.md`) following this skill's full writing instructions (format spine + track calibration + writing stance) — but skip the vault pipeline (no index, log, prompt file, or reviews)
3. Compare the regenerated lesson against the golden reference in `gurukul/testing/<track>/<lesson-name>.md` — check for structural similarity (same sections, same spine), similar scope and depth, and consistent tone
4. Report the comparison to the user
5. Keep the temp file for user review. Tell the user the path so they can inspect it. Only delete when the user confirms.
