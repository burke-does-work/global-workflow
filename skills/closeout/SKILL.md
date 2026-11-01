---
name: closeout
description: Behavioral guide for closing out finished work - reconciling PLAN into SPEC, updating README, retiring SPEC and PLAN, and merging any branch.
disable-model-invocation: true
---

## Entry

Request completion approval from the human before closing out.

---

## Reconcile `PLAN.md` into `SPEC.md`

- Read the decisions recorded during implementation in PLAN and name where the built thing diverged from what the spec asked for, and why, before updating anything.
- Update SPEC to describe what was built.

Stop for human review.

---

## Folding

`SPEC.md` folds into three places: `README.md`, `docs/DESIGN.md`, `docs/ADR.md`. If the files do not exist but are required, then create them.

Update `README.md` to document the current state of the system. `README.md` is the highest level overview and project entry point. Content that does not fit README's role goes to the reference document that serves it.

Update `DESIGN.md` with specification that has outgrown the README.

Update `ADR.md` with critical decisions. The ADR shows a later reader why the project has its shape. A decision is critical only if reversing it would mean reworking the project's structure, such as the choice of implementation language. Expect few of these per project, and leave everything else out. Propose entries for human review before writing them.

---

## SPEC and PLAN delete

**Deleting SPEC and PLAN is the human's call.** The AI system does not initiate it.

Once human initiates, commit the final version of SPEC and PLAN, if not the latest, then remove.

---

## Branch merge

Where the work is on a branch, merge it to `main` once SPEC and PLAN are deleted:

- If `main` has moved, merge `main` into the branch first, so the final merge fast-forwards.
- Fast-forward `main` to the branch.
- Delete the local branch with `git branch -d`, which refuses to delete unmerged work.
- Leave pushes and remote branch deletion to the human.
