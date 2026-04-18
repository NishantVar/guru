### Goal 

The goal of this series is to develop self-sufficient AI practitioners, from beginner to expert, who can use AI to materially improve real work. It is taught from first principles so learners build durable understanding, transfer that understanding across tools and tasks, and apply AI not just to generate output, but to automate workflows, build useful systems, and do real engineering with judgment.

### Writing Stance

Write lessons like a cheat sheet with context, not a tutorial.

The lesson should make the learner more capable quickly. It should not walk them through every tiny step, narrate every screen, or explain obvious defaults unless that detail changes the learner's judgment.

### Hard Requirements

#### 1. The first paragraph must carry the lesson

Assume many readers will only read the first paragraph.

That paragraph must tell them what the lesson is about, why it matters, and enough of how it works to give them a usable mental model.

It should prime the learner for the rest of the lesson, not merely introduce the topic.

A practical test: if someone pastes only the first paragraph into an agent, that agent should have enough context to help them work through the rest of the lesson.

#### 2. Each lesson should teach one core idea

Every lesson should have one main job.

Supporting points are fine, but they must serve the central idea rather than compete with it.

If the lesson cannot be summarized in one plain sentence, it is probably trying to do too much.

#### 3. Give an early concrete anchor

In the first section, give the learner something tangible as early as possible.

This can be an example, a command, a setup shape, a success state, a reference table, or a minimal workflow.

Do not make the learner wait through abstract explanation before they can picture the thing.

#### 4. Include a decision rule and a failure mode

The lesson should not only explain what the thing is. It should also help the learner judge when to use it and where it breaks.

Include at least one clear decision rule, tradeoff, limitation, or common mistake.

This is often where the real value of the lesson lives.

#### 5. Make the learner do something

Every lesson should include at least one active step.

The learner should try a prompt, inspect an output, predict a result, answer a question, fix something broken, or apply the idea to their own workflow.

Passive reading alone is not enough.

### Default Lesson Structure

Use this as the default spine for most lessons.

It is not a rigid template, but the lesson should usually follow this order unless there is a clear reason not to.

#### 1. Opening paragraph

The first paragraph should do the heavy lifting.

It should cover the what, the why, and enough of the how to orient the learner.

#### 2. Early concrete anchor

Give the learner something tangible as early as possible.

This can be an example, a command, a setup shape, a success state, a table, or a minimal workflow.

For setup lessons, the anchor may be what a working setup looks like rather than a worked example.

#### 3. Core model

Explain the simplest useful mental model for how the thing works.

Keep it practical and short.

#### 4. Decision rule

Say when to use it, when not to use it, and what problem it is actually good at solving.

#### 5. Failure mode

Include at least one common mistake, limitation, or way people misuse the idea.

#### 6. Try this now

Give the learner a small active step so they apply the idea immediately.

#### 7. Short recap

If the lesson benefits from a closing line, keep it brief and action-oriented.

### Core Rules

#### 1. Write like a cheat sheet with context

Default to high information density, clear structure, and direct usefulness.

Explain what matters, why it matters, and how to use it in practice.

Do not expand into a long guided walkthrough unless the topic genuinely requires it.

#### 2. Trust the reader

Assume the reader can infer basic steps and does not need every transition spelled out.

Include the parts that improve capability: commands, examples, tradeoffs, failure modes, decision rules, and patterns that transfer.

Cut filler such as UI narration, obvious confirmations, or exhaustive branch coverage.

#### 3. No hand-holding

Do not write in a babysitting tone.

Avoid lines that merely reassure, over-explain, or narrate what the learner will obviously see or do next.

Give enough context to make the reader effective, then move on.

#### 4. Prefer tables and code blocks over prose

When information can be made clearer as a table, list, command block, or compact example, use that form.

Use prose for interpretation, judgment, and framing, not for padding.

Dense reference formats are usually better than long explanatory paragraphs.

#### 5. Keep sections short and easy to scan

Use short sections, front-loaded sentences, and small blocks that work well in Discord.

Avoid walls of text and sprawling sections.

Do not optimize for a rigid line count. Optimize for clarity, pacing, and scanability.
