---
name: walkplan
description: Human-paced walkthrough of an existing PLAN file; agent writes only on request. Load when the human invokes /walkplan. Not for drafting a plan, and not for autonomous or gated implementation.
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
