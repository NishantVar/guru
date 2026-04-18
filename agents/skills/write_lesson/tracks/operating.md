# Track Calibration: Operating

## Core Learner Shift

Move from natural use alone to using better tool environments, connected systems, and codified defaults that make the agent more capable.

## What Lessons Should Optimize For

- Understanding where tool leverage comes from
- Choosing the right environment or integration
- Getting repeatable benefit from setup
- Using system help without needing deep internals

## Authoring Direction

- Teach the workflow shape around the agent, not just the tool
- Show how connected systems change what the agent can actually do
- Explain why the setup is worth the overhead
- Keep the learner focused on outcome, not hidden mechanics
- Projects should end with a durable workflow, not just a completed task
- **Prompts can carry more context than vibing.** The learner may need to describe a desired state, specify what to connect, or tell the agent what configuration to aim for. But the agent still figures out the *how*. If the prompt needs to specify implementation steps to work, either the task is too advanced or the lesson should reframe it as a desired outcome.
- **Every setup needs a "why now" and a "what changes."** Vibing lessons can get away with "try this, it's cool." Operating lessons must answer: why is this setup worth the one-time cost? What gets easier *after* the setup is done? Show the before/after.

## What Success Looks Like

The learner can use a stronger environment, plugin, MCP server, persistent instruction system, or connected workspace to raise the baseline quality of what the agent can do.

## Good Lesson Shapes

- Environment and tool leverage lessons
- Connected capability lessons
- Setup-plus-usage labs
- Small projects that create a reusable personal workflow

## Avoid

- Pretending tool access equals maturity
- Dropping into unnecessary implementation detail
- Requiring the learner to design the system from scratch
- Assuming explicit verification habits that belong more to Directing
- Re-teaching vibing concepts (e.g., don't re-explain what MCP is — the vibing lesson already covered that. Trust the prerequisite chain.)
- Pure tool showcases with no workflow payoff — if the lesson is "install X and see that it exists," it belongs in vibing

## Track-Specific Failure Mode

Mistaking good tools for clear thinking. The learner sets up powerful integrations but doesn't know when to use them, resulting in over-tooled workflows that add complexity without real leverage.

## Lesson Admission Criteria

A lesson belongs in operating only if **all** of these are true:

| # | Gate | Test |
|---|------|------|
| 1 | **Setup creates lasting leverage** | Does the learner end up with something that improves *future* work, not just a one-time result? If the payoff is a single output, it's a vibing lab. |
| 2 | **The environment is the lesson** | Is the topic about the tool, integration, or system *around* the agent — not just a task the agent performs? |
| 3 | **Completable with guided setup** | Can a non-developer follow the setup with their agent's help, even if it takes ~30-45 minutes? |
| 4 | **No system design required** | Does the learner configure or connect things, not architect them? If they need to decide structure, boundaries, or verification criteria, it's directing. |
| 5 | **The "why" is necessary** | Does the lesson *need* to explain why this setup is worth the overhead? If "it works" is sufficient motivation, it might still be vibing. |
| 6 | **Single integration focus** | Is the lesson about one environment change, one integration, or one workflow improvement? If it requires wiring multiple systems into a coherent workflow, it's a project — and if it requires designing the coordination, it's directing+. |

### Post-Writing: Flag Dependencies

Operating lessons have real dependencies — unlike vibing, where lessons should be followable cold. A learner who hasn't set up their agent or doesn't understand MCP will hit walls.

After writing an operating lesson:
- List vibing prerequisites explicitly (e.g., setting-up-coding-agents, mcp-connecting-agents-to-services)
- List operating prerequisites if the lesson builds on another operating setup
- Don't gate on more than 2-3 prerequisites — if a lesson needs more, it's either too advanced or should be split

### Quality Signals

| Signal | Operating fit | Not operating |
|--------|-------------|--------------|
| Learner sets up a tool/integration and uses it in the same lesson | Yes | — |
| Setup creates a durable improvement the learner will reuse | Yes | — |
| The lesson explains *when* to use the setup, not just *how* | Yes | — |
| Learner could do this without any persistent setup | — | That's vibing |
| Learner needs to design system architecture | — | Too advanced (directing+) |
| Learner needs to define explicit success criteria before starting | — | That's directing |
| The topic requires coordinating multiple agents | — | Too advanced (delegating+) |
| The lesson is about connecting agents to systems | Yes | — |
| Understanding "why this helps" is part of the lesson | Yes (with simple explanation) | No (deep architectural reasoning belongs higher) |
| Failure mode is "set up wrong" or "used wrong tool for the job" | Yes | — |
| Failure mode is "built the wrong system" | — | That's directing+ |

### Meta-Test

Ask: **"Would this lesson make someone who's been vibing feel like they just unlocked a capability they didn't have to earn through expertise?"**

- If yes → operating.
- If the learner feels like they need to understand the internals to benefit → rewrite to focus on outcome, or move to a higher track.
- If the lesson could work just as well as a one-off vibing prompt → it's vibing, not operating.
- If the learner needs to make real design decisions to succeed → it's directing.

## Recommended Format Emphasis

| Format | Priority |
|--------|----------|
| `lab` | Primary — tool leverage plus setup-and-try |
| `concept` | Secondary — understanding where leverage comes from |
| `project` | Regular — creates durable personal workflows |
| `drill` | Rare — not enough operational patterns to drill yet |
| `case-study` | Rare — save for later tracks with richer judgment needs |
