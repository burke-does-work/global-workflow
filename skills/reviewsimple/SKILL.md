---
name: reviewsimple
description: Simplicity review of a finished branch - is this more than the job needs, and can the human read all of it. Load only when the human invokes /reviewsimple.
---

One question: is this more than the job needed, and can the human read all of it.

Invoked explicitly, in a fresh session, on a finished branch. Not during the work.

---

## Do not read the plan

Read the diff, `/code-guide`, and `/style-guide`. Nothing else.

The plan and the spec explain why each piece exists, which is exactly what makes complexity look justified. A reviewer holding the rationale stops seeing the thing being rationalised. Read the code as someone who has to maintain it and was not there when it was written.

If a piece of code cannot be understood without the plan, that is a finding rather than a reason to go and read the plan.

---

## Report, do not fix

Findings only. Edit nothing.

The human decides what to act on. A finding that gets rejected is not a failure of the review - half of them should be, because the judgment being applied is theirs.

---

## The standard

`/code-guide` defines readable for this human. Beginner to intermediate Python and SQL, explicit over implicit, no clever abstractions or complex one-liners. Measure against that, not against what is readable in general.

Read `/code-guide` before starting so a deliberate house convention is not reported as a problem.

---

## What to look for

- Abstraction with one caller. A function, class, or module extracted before a second use existed.
- Configuration for something that does not vary. Parameters, flags, and settings with one real value.
- Defensive code past the boundary. `/code-guide` asks for validation on entry and simple logic after it. Repeated checks downstream are the failure it names.
- Indirection that adds a hop without adding a decision. Wrappers, passthroughs, a layer that only forwards.
- Generality nobody asked for. Handling cases the spec does not contain.
- A dependency carrying a small amount of work.
- Anything the human would have to ask a senior developer with domain expertise to explain. That is the whole test, stated plainly.
- Code that, after implementation, carries a lot of weight but only adds limited feature or structural value.

---

## What a finding says

- Where it is.
- What is more than the job needed.
- What the simpler version would be, concretely enough to act on.

If the simpler version cannot be stated, the finding is not ready. "This feels complex" is not a finding.

---

## Ordering

Lead with the finding that removes the most code. Simplicity findings compound - an abstraction removed often takes its configuration and its tests with it - and the largest one frequently makes the smaller ones moot.

State plainly when there is nothing to report. A short review is a real outcome, not a failure to find something.
