# Track Calibration: Vibing

## Core Learner Shift

Move from vague curiosity or passive use into natural, confident use of a coding agent for visible tasks.

## What Lessons Should Optimize For

- Reducing intimidation
- Visible wins in one sitting
- Direct usefulness
- Tool trust without false certainty
- Simple mental models

## Authoring Direction

- Lead with the concrete outcome fast
- Use one tool or one idea at a time
- Keep jargon light and define it immediately
- Prefer labs, small concepts, and compact projects
- Make "try this now" easy enough to do immediately
- **Prompts should be simple and obvious.** Plain language, no clever structure, no multi-part instructions. The learner should almost be able to guess the prompt without the lesson. It's fine if the first attempt doesn't produce a perfect result — iteration is expected, not a failure. If the prompt needs to be clever to work, the task is too advanced for vibing.

## What Success Looks Like

The learner can ask an agent to do something useful, recognize whether the result basically worked, and understand one simple rule about when to use the capability.

## Good Lesson Shapes

- Setup lessons
- Simple capability intros
- Low-stakes projects with one visible deliverable
- Confidence-building reference maps

## Avoid

- Long setup chains before any payoff
- Dense theory before a concrete example
- Requiring the learner to define tests or architecture
- Multi-agent coordination
- Re-explaining concepts covered by prerequisites (e.g., don't define "coding agent" — the setup lesson already did that. Trust the prerequisite chain.)

## Track-Specific Failure Mode

Confusing plausible output with correct output. The learner accepts what the agent produces because it looks right, without knowing how to verify or question it.

## Lesson Admission Criteria

A lesson belongs in vibing only if **all** of these are true:

| # | Gate | Test |
|---|------|------|
| 1 | **Single concept or single task** | Can you state what the learner gets in ≤20 words? If you need "and" to describe it, it's two lessons. |
| 2 | **Completable in one short sitting** | Can a non-technical person finish in ~15-25 minutes, including setup friction? |
| 3 | **Visible outcome** | Does the learner have something they can see, show, or use when done? "You now understand X" doesn't count. |
| 4 | **No multi-agent coordination** | Does it use one tool at a time? |
| 5 | **Non-technical person can follow** | Can someone who doesn't write code get through it without unexplained jargon? |
| 6 | **Simple, obvious prompt** | Could the learner guess a reasonable version of the prompt without the lesson? If it needs careful wording to work, the task is too advanced for vibing. |

### Post-Writing: Flag Dependencies

After writing a vibing lesson, note which other lessons it builds on or benefits from. Vibing learners explore non-sequentially — they should be able to follow any lesson cold, but it's helpful to surface connections. List dependencies in the lesson's `prerequisites` frontmatter and briefly mention them in the opening if context from another lesson would help (e.g., "If you haven't set up a coding agent yet, start with [[gurukul/lessons/vibing/setting-up-coding-agents]]"). Don't gate the lesson on them.

### Quality Signals

| Signal | Vibing fit | Not vibing |
|--------|-----------|------------|
| Learner's main job is following a prompt and checking the result | Yes | — |
| Learner needs to design something (architecture, tests, schema) | — | Too advanced |
| Lesson teaches when to use something | Yes (simple decision rule) | No (complex judgment with tradeoffs) |
| Failure mode is "it didn't work" | Yes | — |
| Failure mode is "it worked but produced subtly wrong output" | — | That's calibration — operating+ |
| The lesson is about setup/installation | Yes, if payoff is in the same lesson | No, if it's pure plumbing with no win |
| The topic requires understanding why something works internally | — | Vibing teaches that it works, not why |

### Meta-Test

Ask: **"Would this lesson make someone who was intimidated by AI tools feel like they just did something real?"**

- If yes → vibing.
- If the lesson makes them feel informed but not empowered → higher track or needs restructuring.
- If the lesson makes them feel capable but overwhelmed → scope is too big, split it.

## Recommended Format Emphasis

| Format | Priority |
|--------|----------|
| `lab` | Primary — confidence and concrete wins matter most |
| `concept` | Secondary — simple mental models |
| `project` | Occasional — small, one-deliverable builds only |
| `drill` | Rare — not enough foundation to drill yet |
| `case-study` | Rare — judgment needs more context than this level has |
