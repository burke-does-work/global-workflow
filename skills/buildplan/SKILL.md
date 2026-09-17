---
name: buildplan
description: Behavioral guide for turning an approved SPEC into a PLAN of implementable slices. Load only when the human invokes /buildplan.
---

Invoked explicitly, never loaded automatically. This skill is deliberately constraining and most planning work does not want it. If the human has not named it, do not apply it.

---

## Scope

Write `branch_work/PLAN.draft.md` and nothing else.

No source files, no configuration, no fixes to problems noticed along the way. Note them in the plan as slices or as open questions instead.

Nothing enforces this. The backstop is git: plan from a clean tree, and the changed-files list at the end either holds the plan alone or it does not. A plan reverse-engineered from code already written is worthless, because it is then checked against the work that produced it.

---

## Entry

- Start from a clean tree. If it is dirty, say so and stop.
- Read the spec. It is the target state and the plan is the route to it.
- Read the project `README.md`, including its `## Verify` section. That names the check the slices have to satisfy.
- Research the code before proposing anything. Dependency order comes from what is there, not from what sounds reasonable.

---

## Slices

A slice is a vertical cut: one user-visible capability, end to end, including the check that proves it. Not a horizontal layer - a layer cannot be verified until the last one lands.

**Sizing is one verification check per slice.** Two checks means two slices. The check is built in the slice, not merely named by it.

**The first slice is the walking skeleton** - the thinnest end-to-end path that does something real. It proves the environment sound and the check runnable before anything depends on either. Where no test suite exists yet, this is what bootstraps the first check.

**Order by dependency, then by uncertainty.** Dependency order is derivable from the code. Uncertainty is not, so state which slice carries the most and why, letting the human react to a claim rather than generate one.

**Each slice carries two fields:**

- A subject line, which is also its commit message. Written and approved at plan time, not drafted per commit.
- A check.

Two markers track it: `(done)` and reviewed.

Nothing else. No intent line separate from the subject - at this size the subject is the intent. No list of files expected to change - cross-slice edits are normal, so the list would be wrong and an out-of-scope edit is caught at review anyway.

### Human review

Individual slices will be reviewed by human after the final agent verification, unless there's a stop work. Include the list of checks required by the human (files and/or behavior) to guide human review.

Overall coherence is checked there too - whether the finished work still serves the goal, not only whether each slice met its check. The review passes and the slice checks are local and cannot see it. This is on top of the agent's own verification, not in place of it.

---

## Size

A plan holds roughly five to ten slices - what the human can review in one sitting. Six slices is six lines and six checks plus the ordering and one uncertainty claim, which is a page.

Work larger than that means the spec is too big. Say so. Do not lengthen the plan or split it in two; there is one spec and one plan at a time.

---

## Feedback

Questions and assessment are given when asked for, the way spec sessions run. Not volunteered, not withheld by rule.

---

## Exit

The plan is approved by the human, in words, in the prompt. Annotation in the file is never approval.

Do not rename the file on your own reading of an approval. The rename from `PLAN.draft.md` to `PLAN.md` happens on an explicit instruction, and the dated approval line is written at the same time. One approval per document, before implementation starts.

---

## Handoff

Implementation begins in a fresh context reading the plan alone. Do not carry planning context into it - the plan is the handoff, and if it is insufficient on its own, that is a defect in the plan.
