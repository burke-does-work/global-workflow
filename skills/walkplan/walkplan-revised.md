# Proposed Revision: `/walkplan`

**Purpose**: Review document for a proposed change to `~/.kiro/skills/walkplan/SKILL.md`. The skill file itself is unchanged. This exists so the reasoning behind the change is visible alongside the change.

The revision is deliberately small. The body of the current skill works -- the failure was in routing, not behavior. Almost everything below is verbatim from the original.

---

## Proposed skill

```markdown
---
name: walkplan
description: Human-paced walkthrough of an existing PLAN file; agent writes only on request. Load when the human invokes /walkplan or asks to work through a PLAN step by step. Not for drafting a plan, and not for autonomous or gated implementation.
---

A plan session works through an existing PLAN step by step, with the human setting the pace. Look for the relevant PLAN file. If no plan file, then proceed with the plan as it exists in the prompt.

Think of it as a printed plan on the table -- the human is doing the work and jotting notes in the margin as they go. The agent holds the pen when asked.

## On load

Find the relevant PLAN file and read it. Do not summarize it or propose a schedule. Wait for the human to name the first step.

## Scope

This skill governs pace and note-taking. It does not carry implementation or verification behavior.

- It does not draft or restructure the PLAN. That is a planning session.
- It does not implement steps on the human's behalf unless separately asked.
- It does not check acceptance criteria, run gates, or block advancement. Completion is the human's declaration, not a test result.

## Pace

The human advances steps. Do not prompt for the next one. Do not declare steps complete unprompted. Do not suggest moving on.

## Notes

Notes are knowledge captured while doing the work, destined for a compendium. They can include decisions or an explicit approach for this PLAN but written as durable beyond the plan.

- Notes live in a subsection nested one level under the relevant step heading.
- Write notes when the human says to. Do not offer proactively.
- Keep technical specifics. Drop project references. A note should read correctly to someone who doesn't know this project.
- Prefer tables for structured facts (comparisons, definitions, option sets). Use prose only when structure doesn't fit.

## Progress tracking

When the human signals a step is done, mark it with `(done)` appended to the step heading in the plan file. No other comment.

When the human asks what's next, name the next unmarked step from the plan. One line.

## Answering questions during a step

Work through the relevant factors first, then give the answer. If a factor is unknown, name it before concluding. A question asked mid-step is often note-worthy, but human will decide to add it or not.

## No handoff

Do not suggest switching to another mode. The human closes the session.
```

---

## What changed

Two changes. Everything else is untouched.

### The description

Original:

> Behavioral guide for PLAN build sessions. Load when the human invokes /walkplan or asks to work through a PLAN file step by step.

Three problems, in order of how much damage they do.

Agency is absent. "Work through a PLAN file step by step" describes the shape of the session but not who acts. Phased implementation is also, generically, working through a plan step by step. The skill's center of gravity is that the human works and the agent holds still -- and that is invisible from the summary line.

"PLAN build sessions" is ambiguous. It reads as either building the plan or building from the plan. The body settles it in its first sentence, but the body is not what gets read during routing.

No exclusions. There is a plausible adjacent use -- agent-led phased implementation with gates -- and nothing in the line rules it out.

The replacement states agency first, keeps the original invocation trigger, and closes with what the skill is not for.

### A scope section

Added near the top of the body, before `On load`. It states the three things the skill does not do.

This is partly redundant with the new description, and that redundancy is intentional: the description governs routing, the section governs behavior once loaded. A skill can be correctly routed and still have its boundaries misread in a long session.

The third bullet -- no acceptance criteria, no gates, no blocking -- exists because that is the specific claim that was wrong when this came up. `(done)` marking looks like a gate mechanism and is not one.

---

## What did not change

The body's substance, nearly in full: the printed-plan metaphor, the on-load silence, the pacing rules, the compendium standard for notes, the `(done)` convention, the mid-step answering discipline, and the no-handoff rule.

Two mechanical edits only: an em dash converted to `--` per the style guide, and a blank line added under the `Notes` paragraph for consistency with the other sections.

---

## Tradeoff worth naming

The new description is roughly three times the length of the original. Skill descriptions sit in context permanently -- only the summary line is loaded until invocation -- so length here is a standing cost across every session, not a one-time one.

A shorter variant that keeps agency and drops the explicit exclusions:

> Human-paced walkthrough of an existing PLAN. The human works and sets pace; the agent marks steps done when told and writes durable notes on request.

This is about the length of the original and fixes the agency problem, which was the largest of the three. It loses the exclusion clause, which was what would most directly have prevented the misroute.

Recommendation: take the longer version. Nine skills at this length is still negligible against a 200k window, and the exclusions are doing real work given how close `/walkplan`, `/spec`, `/collab`, and `/learn-dev` sit to each other.

---

## Open question

The same audit likely applies to the other skills, since the failure mode is structural rather than specific to this one. Descriptions that state a shape without stating agency will misroute whenever an adjacent use exists. `/collab` and `/learn-dev` look most exposed, since `/learn-dev` is documented as read-only and layered on `/collab` -- a distinction that a shape-only description cannot carry.
