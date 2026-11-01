---
name: walkplan
description: Human-paced walkthrough of an existing PLAN file. The agent writes only on request. Not for drafting a plan, and not for autonomous or gated implementation.
disable-model-invocation: true
---

A plan session works through an existing PLAN step by step, with the human setting the pace. Look for the relevant PLAN file. If no plan file, then proceed with the plan as it exists in the prompt.

Think of it as a printed plan on the table - the human is doing the work and jotting notes in the margin as they go. The agent holds the pen when asked.

---

## On load

Find the relevant PLAN file and read it. Don't summarize it or propose a schedule. Wait for the human to name the first step.

---

## Scope

This skill governs pace and note-taking. It doesn't carry implementation or verification behavior.

- It doesn't draft or restructure the PLAN. That's a planning session.
- It doesn't implement steps on the human's behalf unless separately asked.
- It doesn't check acceptance criteria, run gates, or block advancement. Completion is the human's declaration, not a test result.

---

## Pace

The human advances steps.

- Don't prompt for the next one.
- Don't declare steps complete unprompted.
- Don't suggest moving on.

---

## Progress tracking

When the human signals a step is done, mark it with `(done)` appended to the step heading in the plan file. No other comment.

When the human asks what's next, name the next unmarked step from the plan. One line.

---

## Answering questions during a step

Work through the relevant factors first, then give the answer. If a factor is unknown, name it before concluding. A question asked mid-step is often note-worthy. The human decides whether to add it.

---

## No handoff

Don't suggest switching to another mode. The human closes the session.
