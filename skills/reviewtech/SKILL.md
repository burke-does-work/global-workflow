---
name: reviewtech
description: Technical correctness review of a finished branch -- does the code do what it claims. Load only when the human invokes /reviewtech.
---

One question: does the code do what it claims.

Invoked explicitly, in a fresh session, on a finished branch. The independence comes from not having written the code, not from withholding context.

---

## Read the plan

Read the diff, `PLAN.md`, `/code-guide`, and `/style-guide`.

Correctness cannot be judged without intent. Code that is internally consistent and does the wrong thing looks fine in isolation, and the plan is what makes the difference visible.

This is the opposite of `/reviewsimple`, which runs without the plan on purpose. Both passes are needed because each is blind to what the other sees.

---

## The strongest check available here

Each slice states a check. For every slice, compare three things:

- What the slice claimed it would do.
- What the code actually does.
- What the check actually verifies.

A check that passes while the slice's claim is unmet is the most valuable finding this pass can produce, and nothing else in the workflow catches it. The tests are green, the plan is marked done, and the thing does not work. Look for checks that assert something narrower than the claim, assert on a mock rather than the behaviour, or would pass against an empty implementation.

---

## Report, do not fix

Findings only. Edit nothing. The human decides what to act on.

---

## What a finding says

- Where it is.
- What is wrong.
- A concrete failure: specific inputs or state, and the wrong output or crash that results.

If the failure cannot be stated concretely, it is a suspicion rather than a finding. Say so and mark it as such, or leave it out. A list padded with maybes is how a real finding gets missed.

---

## What to look for

- The claim-versus-check gap above. Start here.
- Error paths. `/code-guide` asks for validation at the boundary and simple logic after it, so check that the boundary actually validates what the rest of the code assumes.
- Edge cases the plan implies but the code does not handle -- empty inputs, missing files, absent configuration, a first run with no prior state.
- Assumptions that hold in one slice and not across the branch, where a later slice changed something an earlier one relied on.
- Resource handling: files and connections opened and not closed, work that fails partway and leaves state half-written.
- For pandas, the specific traps `/code-guide` names -- chained assignment, views against copies, silent dtype changes.

---

## What not to report

- Style, formatting, and naming. Prettier, ruff and markdownlint own those, and what they do not own is in `/code-guide` for a human to apply.
- Complexity and overbuilding. That is `/reviewsimple`.
- House conventions. Read `/code-guide` first so a deliberate choice is not reported as a defect.

---

## Ordering

Most severe first, where severity is how wrong the output is and how likely the path is to run. A silent wrong answer outranks a crash, because a crash announces itself.

State plainly when there is nothing to report.
