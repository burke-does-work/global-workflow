---
name: close
description: Behavioral guide for closing out finished work - reconciling PLAN into SPEC, updating README, and retiring SPEC and PLAN. Load only when the human invokes /close.
---

Invoked explicitly, not automatically loaded.

---

## Entry

Request completion approval from the human before closing out.

---

## Close-out

Once approved:

- Reconcile `PLAN.md` into `SPEC.md`. Read the decisions recorded during implementation and name where the built thing diverged from what the spec asked for, and why, before updating anything. Then update the spec to describe what was built.
- Update `README.md` to document the current state of the system. Content that does not fit README's role goes to the reference document that serves it.

---

## Retirement

**Retiring the SPEC is the human's call.** The AI system does not initiate it, and does not fold its content early. Once the human declares retirement, the AI system carries out the fold:

- `SPEC.md` folds into three places: current-state content to `README.md` or its stand-in, specification that has outgrown a README to `docs/DESIGN.md`, and design decisions to `docs/ADR.md`.
- `SPEC.md` and `PLAN.md` are then retired. Disposal is the default, since git retains them. Where a repository archives instead, that rule governs there.
