# Design records

Key decisions and the reasoning that forced them. Entries are immutable. A reversed decision is a new entry naming the one it supersedes.

---

## 2026-09-17 - Formatter and linter split, and records kept separate from design

Enforcing the table style in `/style-guide` required a formatter: `MD060` in aligned mode detects misalignment and has no fixer, so the check failed and stayed failed over table whitespace. Prettier had been rejected earlier for reflowing prose, and a ninety-line alignment script was written instead. That script was a narrow rebuild of Prettier, and `proseWrap: "preserve"` removes the original objection entirely.

Prettier formats every file type it supports and markdownlint lints markdown, wherever either lives. Adopting the formatter made the linter config smaller rather than larger, because every overlapping rule comes out to stop the two fighting. Ruff plays the same role for Python, scoped to mechanical rules so that anything needing judgment stays in `/code-guide` and the review passes.

Rejected: the hand-written alignment script. Rejected: excluding markdownlint from Python projects on toolchain-sprawl grounds, which contradicted the consistency objective and left nine markdown files unlinted in `shop-system`.

`ADR.md` and `docs/design.md` stay separate because they have opposite mutability. A design document describes current state and is rewritten; records are immutable once written. Putting immutable entries at the end of a rewritable file invites them being edited, which is the one thing the records rule forbids.

---

## 2026-09-17 - Two review passes, deliberately opposite

A single review pass cannot do both jobs, because each is blind to what the other sees. A reviewer holding the plan stops seeing the complexity the plan rationalises. A reviewer without it cannot judge correctness, since code that is internally consistent and does the wrong thing looks fine in isolation.

`/reviewsimple` runs without the plan. `/reviewtech` runs with it. Both read `/code-guide` and `/style-guide` for opposite reasons: simplicity is measured against those conventions, and correctness reads them so a deliberate house convention is not reported as a defect. Both report findings and apply nothing.

Rejected: four passes. Automated checks as a pass, since they already run per slice. Subagents, because a fresh session is agent-agnostic and the independence comes from not having written the code. The harness's own `/simplify` and `/code-review`, which apply generic conventions rather than these.

Coherence was cut from the passes because it belongs at close-out, the one point where the spec and the finished work are both present. Both passes are local, one per artifact and one per slice, and neither can see whether the sum still serves the goal.

Downside accepted: a skill's judgment cannot be checked mechanically. The checks verify that a skill runs and stays in scope, not that its findings are good.

---

## 2026-09-17 - Verification is declared, not enforced

The failure being guarded against is not a failing check. It is a check that was never runnable: wrong environment, missing tool, or one that can only pass somewhere the code has not reached. Every enforcement option available was tied to one harness, which the agent-portability constraint rules out.

The project declares its check in `README.md` under `## Verify`, mirroring the existing `## Commits` lookup. The check is defined during spec and plan, and proven runnable on the walking skeleton before anything depends on it. Nothing forces it to run.

Planning is constrained the same way. `/buildplan` states that planning writes the plan and nothing else, and git supplies the tripwire: plan from a clean tree, and the changed-files list afterwards either holds the plan alone or it does not. This is detection rather than prevention, which is sufficient because a backfilled plan is only harmful when it goes unnoticed.

Rejected: a `Stop` hook and a `PreToolUse` hook, both harness-specific. A path-scoped permission rule, which is harness-specific, needs toggling between stages, and is not expressible anyway, since Claude evaluates deny before allow and allow cannot carve an exception out of a deny. A tracked pre-commit hook via `core.hooksPath` is agent-agnostic and deferred rather than rejected.

Downside accepted: a check can silently go un-run and nothing will say so. The exposure is bounded because the check lives in the slice, so an un-run check shows as an unmarked slice rather than disappearing.

---

## 2026-09-17 - Agent commits once per slice, and the merge preserves them

The question was whether agent commits are churn to be discarded or meaningful units to be kept, and it turned on where the boundary comes from. Commit boundaries are fixed during planning, before any code exists. The agent therefore executes a boundary rather than choosing one, which is what makes an unreviewed commit structurally trustworthy and makes squashing a real loss rather than tidying.

The agent commits once per completed slice, with nothing sub-slice and no checkpoints. `main` is merged into the branch during work so the final merge fast-forwards, and nothing is rewritten after review. Review happens once over the finished branch, commit by commit in plan order, read in the human's own editor.

Rejected: squash, which discards commits worth keeping. Sub-slice checkpoints, which buy rollback inside the smallest meaningful unit. No agent commits at all, the only option preserving the staging-as-ledger review mechanism, but which leaves no rollback point when an unattended run goes wrong. Granular commits reshaped into slices before merge, which is fiddly and easy to skip. Review through a pull request, too easy to click through.

Downside accepted: `main` carries agent-written messages reviewed in aggregate rather than one by one, and the staging ledger does not survive, because the tree is clean at review time.

`ai-dlc` requires a plain merge at its top tier so the reviewed commit survives as a direct parent. That matters where the reviewer is not the merger and an audit trail must prove the link mechanically. Here they are the same person in one sitting, so the guarantee is redundant except when review and merge fall weeks apart, which the state file covers.

---

## 2026-09-17 - The slice is the unit of work, and it carries two fields

The slice had to serve four jobs at once: one commit, one rollback point, one review unit, and one future parallel-agent assignment. Any sizing rule that served three and broke the fourth was not usable, which is why the slice definition ended up upstream of most of the git decisions.

A slice is a vertical cut: one user-visible capability end to end, including the check that proves it. Sizing is one verification check per slice, and the check is built in the slice rather than named by it. The first slice is the walking skeleton, the thinnest end-to-end path that proves the environment sound and the check runnable. Slices are ordered by dependency, then by uncertainty.

Each slice carries two fields: a subject line, which is also its commit message, approved at plan time rather than drafted per commit; and a check.

Rejected as the sizing rule: no conjunction in the commit subject, which is gameable by writing a vague subject. It survives as a secondary smell rather than the test. Rejected as fields: a separate intent line, because at this size the subject is the intent; and a list of files expected to change, because cross-slice edits are normal so the list would be wrong, and an out-of-scope edit is caught at review anyway. That second one earns its place later, for bounding a parallel agent's context.

---

## 2026-09-17 - One SPEC and one PLAN, held in files rather than session state

The workflow has to survive three-week gaps. An unenforced practice quietly stops happening across an interruption, and the value of writing it down is memory rather than scale. Branch-scale planning also routinely spans a close and reopen of the harness.

One `SPEC.md` and one `PLAN.md`. A spec on a branch becomes a plan. Two live specs means nothing states what the system is being built toward; two live plans means nothing states what is being done now.

`PLAN.md` carries the plan rather than harness plan mode. Plan mode holds the plan in session state, so it would have to be reconstructed on every reopen. The second benefit surfaced later: with a file, nothing depends on the human remembering to ask for the plan to be written before implementation starts. Plan mode still earns its place for in-session work, and its `ctrl+g` handoff into an editor is what makes the annotation convention usable.

Approval is recorded in two places, the filename and a dated line inside. The filename carries it because of the walk-away case: planning often finishes with no time left for implementation, and weeks later a directory listing has to answer whether the document is still open without anything being opened.

Downside accepted: outside plan mode there is no write block, and renames churn git history and stale any path reference elsewhere. The first is covered by the git tripwire, the second by git's rename detection.
