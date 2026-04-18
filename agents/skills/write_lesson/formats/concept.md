# Format: Concept Lesson

**When to use:** The learner needs a stable mental model, principle, or judgment rule. Use when the user asks to "explain," "teach," or describes a single concept. The learner leaves with understanding and judgment, not a built artifact.

## Structure Spine

Use this as the default spine. It's not rigid — deviate when there's a clear reason.

### 1. Opening paragraph

This paragraph carries the lesson. Assume many readers stop here.

It must cover what the lesson teaches, why it matters in real work, and enough of the how to give a usable mental model. Practical test: if someone pasted only this paragraph into an agent, the agent should have enough context to help them work through the rest.

Keep it to 2-4 sentences. Compress rather than cut — weave the what, why, and how into fewer sentences instead of giving each its own. A tight paragraph that covers everything is better than a thorough one that reads like a wall of text. The early concrete anchor right after does the heavy lifting on details.

Don't open with history, definitions, or architecture. Lead with the capability.

### 2. Early concrete anchor

In the first third, give the learner something tangible — an example prompt they can give their agent, a success state, a reference table, a minimal workflow, or a picture of what the finished thing looks like.

This is the "I can follow this" moment. Don't make them wade through abstraction before they can picture the thing. For setup lessons, the anchor might be what a working setup looks like rather than a worked example.

### 3. Core model

The simplest useful mental model for how the thing works. Start with what it does in practice, then explain the mechanism briefly.

Keep it short. Learners need enough to reason about the tool, not a full architecture review.

### 4. Decision rule

When to use it, when not to, and what problem it actually solves.

This is often where the real value of the lesson lives — the judgment that separates someone who knows the concept from someone who can apply it.

### 5. Failure mode

At least one common mistake, limitation, or misuse pattern.

If you used an analogy earlier and it breaks down in an important way, flag it here. Honest treatment of limitations builds more trust and capability than pretending things always work.

### 6. Try this now

An active step — give their agent a prompt, inspect the output, predict a result, answer a question, fix something broken, or apply the idea to their own workflow.

Passive reading is not enough. The learner should do something. Frame the active step as something to tell the agent, not a manual procedure to follow.

### 7. Closing (optional)

If the lesson benefits from a brief closing line, keep it to one or two sentences — action-oriented, not a summary of what was already said. The curriculum structure handles lesson sequencing, so don't add "next step" links or "what's next" sections.

## Scope

One core idea, supported by at most 3-4 related points. If you can't summarize the lesson's value in about 20 plain words, it's trying to do too much.

Cut adjacent interesting material rather than cramming it in.

## Teaching Order

Concept lessons follow concrete -> mechanism -> judgment:

1. **Concrete** — what the thing does in practice (examples, outputs, prompts to try)
2. **Mechanism** — how it works at the simplest useful level
3. **Judgment** — when to use it, where it breaks, what good decisions look like

Build confidence before nuance. Give a stable first mental model before adding caveats and edge cases. But be honest when something is genuinely uncertain.

## Pre-Publish Checklist

| # | Check |
|---|-------|
| 1 | Can the lesson's value be stated in one sentence? |
| 2 | Does the first paragraph say what the learner gets and why it matters? |
| 3 | Is there a quick win early? |
| 4 | Is there at least one active step? |
| 5 | Can a non-technical learner follow without unexplained jargon? |
| 6 | Does it move from concrete -> mechanism -> judgment? |
| 7 | Will it still make sense if today's tool changes next month? |
| 8 | Is the formatting easy to scan on Discord? |
