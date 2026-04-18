# Format: Project Lesson

**When to use:** The learner is building one coherent deliverable. Use when the user asks to "build," "project," "set up X and Y together," or describes a multi-step deliverable. The learner leaves with something functional and the experience of having built it.

## Structure Spine

Use this spine when the lesson produces a working deliverable.

### 1. Opening paragraph

Same rule as concept lessons — lead with the capability, not a description of the lesson. What will the learner have when they're done? Why is it useful? Give enough of the shape that the reader can picture the finished product.

Keep it to 2-4 sentences. Compress rather than cut — weave the deliverable, its value, and the transferable pattern into fewer sentences. The "What you're building" section right after handles the detailed breakdown.

### 2. What you're building

A concrete picture of the end state — what the finished thing does, what it looks like in action, what it connects to. Aim for a "when you're done, you'll have X that does Y" description.

Don't preview the build. If a component gets its own build section, don't describe it here — the learner will meet it in context. The anchor is the outcome shape, not an inventory of parts.

### 3. What you'll need (optional)

Only list non-obvious prerequisites — things that would surprise the learner or block them mid-build if they didn't set up beforehand. Examples: a third-party account, a tool outside the standard coding agent setup, admin access to a specific service, prior lessons that teach concepts this project assumes.

Don't list coding agents, subscriptions, or anything a learner in this curriculum obviously has. If a project has zero non-obvious prerequisites, skip this section entirely.

### 4. The build

Sections organized around meaningful milestones, not micro-steps. Each section should describe the outcome the learner asks their agent to produce, then how to verify it worked. The learner's job is to give good prompts and confirm results — the agent handles execution.

**Show the prompt, not the procedure.** When a build step involves setup, configuration, or construction, give the learner an example prompt to paste into their coding agent. The prompt should describe the desired outcome with enough specifics (names, IDs, permissions) for the agent to act on it — not a step-by-step walkthrough of the underlying commands or UI clicks. The agent figures out the how.

**Give verification, not narration.** After each milestone, tell the learner what to check: "You should now see X in Y" or "Ask the agent to confirm Z." Skip descriptions of what the agent will do along the way — the learner can watch it happen in real time.

Where the build involves concepts worth understanding (not just following), explain briefly why a step matters — but keep it tight. The build is the main event, not a concept lesson in disguise.

### 5. Verify it works

An end-to-end test of the complete deliverable. Specific inputs, expected outputs. Not "try it out" — a concrete scenario that proves the whole thing is wired together.

### 6. What can go wrong

Troubleshooting the most common issues. More operational than concept lessons' failure modes — specific error messages, misconfigurations, and their fixes. Focus on the problems that actually stop people, not theoretical edge cases.

### 7. Make it yours (optional)

2-3 concrete extensions or customizations the learner can try. Not a roadmap — specific modifications that build on what they just made. This gives ambitious learners a next move without turning the lesson into a series.

## Scope

One working deliverable. Multiple concepts can appear in service of the build, but the project should produce one coherent thing. If the project requires building two unrelated systems, it's two projects.

Cut adjacent interesting material rather than cramming it in.

## Teaching Order

Project lessons follow goal -> build -> reflect:

1. **Goal** — what you're building and why it's useful
2. **Build** — step-by-step construction with verifiable milestones
3. **Reflect** — what can go wrong, how to extend it, what principles transfer

## Pre-Publish Checklist

| # | Check |
|---|-------|
| 1 | Is the deliverable clear from the opening paragraph? |
| 2 | Does the first paragraph say what they'll build and why it's useful? |
| 3 | Are non-obvious prerequisites (if any) listed upfront? No obvious stuff like coding agents or subscriptions? |
| 4 | Does each build section produce a verifiable result? |
| 5 | Is there an end-to-end verification step? |
| 6 | Are the most common failure points covered with specific fixes? |
| 7 | Will the project still work if today's tool updates next month? |
| 8 | Is the formatting easy to scan on Discord? |
