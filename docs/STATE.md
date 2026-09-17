# STATE

As of: 2026-09-16

Agent-maintained. Overwritten, never appended -- only the current copy matters. History belongs in `WORK_LOG.md`, decisions in `ADR.md`. Disposed at close-out.

---

## Stage

Implementation complete. Waiting at the sign-off gate before close-out.

Branch: `docs/git-workflow`. `docs/SPEC.md` and `docs/PLAN.md` are both approved, dated 2026-09-16.

---

## Where work stopped

All eight slices in `docs/PLAN.md` are marked `(done)`. Both checks pass: `npx prettier --check` and `npx markdownlint-cli2` exit zero.

Nothing is committed. Every change listed by `git status` is from this work.

---

## Next

- Human reviews the branch and gives implementation sign-off. The plan states that slices passing their checks is not approval.
- On sign-off, close-out runs: spec folds into the skills, `AGENTS.md`, `design.md` and `ADR.md`, then `docs/SPEC.md` and `docs/PLAN.md` are disposed.

---

## Outstanding

- Four items were held at the `AGENTS.md` reconcile and still need sorting at close-out: `PLAN.md` lifetime, the slice definition, the `.draft` convention, and the stop triggers.
- The spec rule excluding markdownlint from Python projects is wrong and contradicts the consistency objective set later. `shop-system` holds nine markdown files. Correct it at close-out or record the correction in `ADR.md`.
- `scratch/PLAN.draft.md` holds a separate, unapproved rollout plan for the other repos. It is not part of this work and runs on its own.
- The three new skills have not been invoked for real. Their checks verify scope, not judgment. That shows on first use.
