# Format: Lab

**When to use:** The learner should try one bounded thing right now and get a visible result. Use when the intent is practical but doesn't require a full project. The learner leaves with a completed task and the confidence that comes from having done the thing.

## Structure Spine

### 1. Opening paragraph

What you'll try, why it matters, and that it's completable in one sitting. Same compression rules as concept lessons — lead with the capability, not a description of the lesson.

### 2. Success state (optional)

What "done" looks like, shown before the learner starts. Include this when the end state isn't obvious from the opening paragraph — e.g., when the task involves setup with multiple moving parts, or when "done" looks different from what the learner might expect. Omit when the task is simple enough that the opening paragraph already makes the outcome clear.

### 3. The why (optional)

Why this task matters beyond just completing it. The deeper motivation — what changes about how the learner works, thinks, or operates after doing this. 2-4 sentences max. If it grows past a short paragraph, the motivation is really a concept that should be its own lesson. Omit when the task is self-evidently useful.

### 4. The task

What the learner does. Usually this is a prompt to give their AI agent, but it could also be a configuration step, a comparison, or an experiment. Be specific enough to act on — the learner should be able to start immediately after reading this section.

### 5. Verify

Concrete observable check that proves it worked. Not "try it out" — a specific thing to look for, test, or confirm.

### 6. Common failure modes

1-3 things that go wrong and how to fix each one. Ordered by likelihood. Only include failures that actually stop people — don't pad with theoretical edge cases. If verification fails (the learner followed the steps but doesn't see the expected result), include that as a failure mode when it's a realistic scenario.

### 7. What you need (optional)

Tools, subscriptions, files, or setup the learner needs before starting. One or two sentences, not a full walkthrough. Omit this section entirely if the learner can start with any standard AI tool.

## Scope

One bounded task, one success state. If it needs multiple sessions, it's a project. If it's mainly teaching a mental model without a try-now task, it's a concept.

## Teaching Order

Success state (if needed) -> why -> task -> verify -> failure modes.

When included, the success state comes first so the learner knows where they're headed. When omitted, the opening paragraph carries that weight. Then motivate why it's worth doing, give the learner the tools to get there, confirm it worked, and handle what goes wrong.

## Pre-Publish Checklist

| # | Check |
|---|-------|
| 1 | If the success state isn't obvious from the opening, is it shown before the learner starts? |
| 2 | Is there a specific task the learner can act on immediately? |
| 3 | Is there a concrete verification step? |
| 4 | Is there at least one failure mode with a fix? |
| 5 | Can the learner finish in one sitting? |
| 6 | Is the formatting easy to scan on Discord? |
| 7 | If "the why" is present, is it under 4 sentences? |

## Anti-Patterns

| Don't | Why |
|-------|-----|
| Bury the success state after the instructions | The learner needs to know where they're going before they start |
| Write a lab that's really a concept lesson with a task tacked on | If the main value is the mental model, use the concept format |
| Require multiple sessions | That's a project, not a lab |
| Pad failure modes with edge cases | Only include failures that actually stop people |
| Let "the why" become a concept lecture | If it needs more than 4 sentences, split the concept into its own lesson |
