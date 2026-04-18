---
title: Lesson Evaluation Rubric
created: 2026-04-11
updated: 2026-04-14
type: reference
tags:
  - teaching-methodology
  - rubric
  - lesson-writing
---

# Lesson Evaluation Rubric

Use this rubric to evaluate whether a written lesson matches The New Order's lesson-writing standards.

## Purpose

A strong lesson makes the learner more capable in real work, not just more informed.

Grade the lesson text itself. Do not infer intent. Quote direct evidence for every judgment. Only pass a criterion when the lesson shows real substance, not surface-level compliance. If a criterion cannot be verified from the lesson, score it lower.

## How To Grade

### Format Routing

Before grading, check the lesson's `content_type` frontmatter field:

- **`content_type: lesson`** — Apply the full rubric below: all 7 hard gates + 9 quality dimensions + claims check.
- **`content_type: briefing`** or **`content_type: reference`** — Do not apply this rubric. Briefings and references are evaluated only against their format-specific checklist in `agents/skills/write_lesson/formats/briefing.md` or `formats/reference.md`. Return a pass/fail against that checklist instead.

### Grading Steps

1. Read the full lesson once before scoring.
2. Check the hard gates first. If a hard gate fails, the lesson is not publish-ready.
3. Score each quality dimension from 0-3 using quoted evidence.
4. Extract any important claims the lesson makes and check whether the lesson actually supports them.
5. End with the smallest set of revisions that would materially improve the lesson.

## Hard Gates

All hard gates should pass before publication.

### 1. First Paragraph Carries the Lesson

Pass when the first paragraph says what the lesson teaches, why it matters in real work, and enough of how it works to give the learner a usable mental model.

Fail when the opening is mostly topic announcement, background, history, definitions, or vague motivation.

Does not count: naming the topic, promising value later, or using a hook without giving a usable model.

### 2. One Core Idea

Pass when the lesson has one main job and can be summarized in one plain sentence. Supporting points should serve the central idea rather than compete with it.

Fail when the lesson is really several lessons at once, or when major sections pull in different directions.

Does not count: a tidy title sitting on top of a scattered lesson.

### 3. Early Concrete Anchor

Pass when the first section gives the learner something tangible early: an example, command, setup shape, success state, reference table, or minimal workflow.

Fail when the lesson makes the learner wait through abstract explanation before they can picture the thing.

Does not count: a vague anecdote, a placeholder example, or a generic statement like "this is useful in many cases."

### 4. Decision Rule

Pass when the lesson gives at least one clear rule for when to use the idea, when not to use it, or what problem it is actually good at solving.

Fail when the lesson explains only what the thing is or how to do it.

Does not count: generic advice such as "use this when helpful" or "it depends."

### 5. Failure Mode

Pass when the lesson includes at least one specific mistake, limitation, tradeoff, or break point that improves learner judgment.

Fail when the lesson presents the idea as universally reliable or includes only empty caution language.

Does not count: generic caveats such as "AI can make mistakes."

### 6. Factual Grounding

Pass when all technical details (commands, feature names, tool behaviors, workflows) are accurate or explicitly flagged as uncertain. The lesson does not invent features, fabricate flags, or present hallucinated details as fact.

Fail when the lesson contains made-up commands, non-existent feature flags, invented tool names, or describes workflows that don't actually work the way described.

Does not count: hedging with "I think" while still presenting fabricated specifics.

### 7. Active Step

Pass when the learner is asked to do something concrete: try a prompt, inspect an output, predict a result, fix a broken example, answer a question, or apply the idea to their own workflow.

Fail when the lesson is passive reading from start to finish.

Does not count: a rhetorical question, a reflection prompt with no concrete task, or an optional exercise disconnected from the lesson's core idea.

## Quality Dimensions

Score each dimension from 0-3.

### 1. Capability-First Framing

`3`: The opening clearly states what the learner will be able to do, why it matters in real work, and avoids leading with background or definitions.

`2`: The opening is useful but one part is weak, delayed, or too abstract.

`1`: The lesson names the topic but the practical payoff is vague or buried.

`0`: The opening is dominated by history, definitions, architecture, hype, or scene-setting.

### 2. Concrete -> Mechanism -> Judgment

`3`: The lesson moves from observable example or action, to the simplest useful mental model, to when to use it and where it breaks.

`2`: All parts are present, but one stage is thin or underdeveloped.

`1`: The lesson is mostly example without explanation, or mostly explanation without usable judgment.

`0`: There is no clear progression from practical use to understanding to decision-making.

**Format-specific interpretation:**
- **Drills**: Interpret as "exercise -> evaluate -> calibrate." The concrete anchor is the exercise, the mechanism is the evaluation criteria, and the judgment is the calibration note.
- **Case studies**: Interpret as "scenario -> analysis -> principle." The concrete anchor is the scenario, the mechanism is the analysis, and the judgment is the transferable decision rule.

### 3. Scope Discipline

`3`: The lesson stays tightly focused on one core idea with no more than a few supporting points.

`2`: The lesson is mostly focused but includes some adjacent material that should have been cut.

`1`: Multiple ideas compete for attention and the lesson feels crowded.

`0`: The lesson tries to cover too much to function as a single lesson.

### 4. Cheat-Sheet Stance

`3`: The lesson is high-density and practical. It trusts the reader, avoids babysitting, says things once in the right place, and includes the details that improve judgment.

`2`: The lesson is mostly direct but still includes some unnecessary narration, hand-holding, or repeated information.

`1`: The lesson reads more like a step-by-step tutorial than a capability-building cheat sheet.

`0`: The lesson is padded with filler, obvious transitions, or exhaustive walkthrough detail that does not improve judgment.

### 5. Durable Understanding

`3`: The lesson teaches principles that transfer across tools and tasks. Current tools are used as examples, not as the curriculum itself.

`2`: The lesson is useful but somewhat tied to a specific product or interface.

`1`: The lesson is mostly tool trivia or UI procedure with limited transfer value.

`0`: The lesson would become weak or obsolete as soon as today's tool changes.

### 6. Clarity and Simplicity

`3`: The lesson uses plain language, defines jargon on first use, and uses examples or analogies carefully without becoming misleading.

`2`: The lesson is mostly clear but contains some unexplained jargon or avoidable complexity.

`1`: The learner must work too hard to decode terminology, phrasing, or examples.

`0`: The lesson is confusing, misleading, or hides weak thinking behind jargon.

### 7. Early Win and Active Learning Quality

`3`: The learner gets a quick "I can follow this" moment early, and the active step directly reinforces the core idea.

`2`: The lesson has an early win or active step, but one of them is weaker than it should be.

`1`: The payoff comes late, or the exercise feels tacked on rather than integral.

`0`: There is no early win and no meaningful learner action.

### 8. Scanability for Discord

`3`: Sections are short, first sentences are front-loaded, and tables, bullets, examples, or code blocks reduce friction.

`2`: The lesson is generally easy to scan but has a few dense stretches or oversized sections.

`1`: The structure exists but the lesson still feels text-heavy or slow to parse.

`0`: The lesson is a wall of text or otherwise hard to scan quickly.

### 9. Tool Breadth

`3`: The lesson teaches principles that transfer across tools. Uses one tool (preferably Claude) as the primary example but acknowledges alternatives and shows how the idea applies broadly.

`2`: The lesson is mostly transferable but leans heavily on one product without acknowledging alternatives.

`1`: The lesson is useful only if you use one specific tool, with limited transfer value.

`0`: The lesson is product documentation disguised as a lesson.

## What Not To Reward

Do not give extra credit for:

- sounding smart instead of making the learner capable
- dropping jargon as a shortcut for explanation
- adding a token exercise that does not test the lesson's main idea
- attaching a generic caveat and calling it a failure mode
- including tool names, product screenshots, or UI narration without transferable understanding
- fabricating technical details that sound authoritative but are invented
- writing a lesson that only works for one product

## Claims Check

After scoring, extract important claims the lesson makes and verify whether the lesson earns them.

Examples:

- "After this lesson, you will know when to use X"
- "This pattern transfers across tools"
- "This approach is more reliable"
- "This setup is enough to start building"

If the lesson makes a claim that the content does not support, flag it even if the surrounding prose sounds strong.

## Overall Verdict

Use this decision rule:

- `Publish-ready`: all hard gates pass, total score is `20-27`, and no quality dimension scores `0`
- `Needs revision`: all hard gates pass, but total score is `15-19`, or one quality dimension scores `0`
- `Not ready`: any hard gate fails, or total score is `0-14`

Note: there are 7 hard gates and 9 quality dimensions (max score 27).

When uncertain, the burden of proof is on the lesson.

## Evaluation Template

Use this format when grading:

```md
# Lesson Evaluation

## Hard Gates

1. First paragraph carries the lesson: PASS/FAIL
Evidence: "..."

2. One core idea: PASS/FAIL
Evidence: "..."

3. Early concrete anchor: PASS/FAIL
Evidence: "..."

4. Decision rule: PASS/FAIL
Evidence: "..."

5. Failure mode: PASS/FAIL
Evidence: "..."

6. Factual grounding: PASS/FAIL
Evidence: "..."

7. Active step: PASS/FAIL
Evidence: "..."

## Quality Dimensions

1. Capability-first framing: 0-3
Evidence: "..."

2. Concrete -> mechanism -> judgment: 0-3
Evidence: "..."

3. Scope discipline: 0-3
Evidence: "..."

4. Cheat-sheet stance: 0-3
Evidence: "..."

5. Durable understanding: 0-3
Evidence: "..."

6. Clarity and simplicity: 0-3
Evidence: "..."

7. Early win and active learning quality: 0-3
Evidence: "..."

8. Scanability for Discord: 0-3
Evidence: "..."

9. Tool breadth: 0-3
Evidence: "..."

## Claims Check

- Claim: "..."
Supported: YES/NO
Evidence: "..."

## Verdict

- Total score: X/27
- Final verdict: Publish-ready / Needs revision / Not ready

## Highest-Leverage Revisions

1. ...
2. ...
3. ...
```
