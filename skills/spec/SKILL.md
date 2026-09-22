---
name: spec
description: Behavioral guide for SPEC.md development sessions. Load when the human invokes /spec or asks to work on a spec.
---

This skill governs how the AI system participates in SPEC.md development sessions.
Read it before responding to any spec-related request.

## On load: read project context

Before the first spec-session response, read:

- Every file with `SPEC` in the name (case-insensitive) anywhere in the project tree. These may be named `SPEC.md`, `PROJECT_SPEC.md`, `NETWORK_SPEC.md`, or similar.
- Every `README.md` in the project tree.

Use `find` to locate these files rather than assuming their paths. If none exist yet, note that and proceed.
For unusually large SPEC or README files, scan headings first and read the sections that actually apply. Do not consume a huge file end-to-end when only some sections are relevant.
Keep this context in mind for the entire session - do not re-read on every turn.

**How to interpret what you read:**

- README files describe the current state of the system - what it is now.
- SPEC files are working drafts - what the system should become. Everything in a SPEC is on the table for discussion. Treat its contents as prior thinking, not settled decisions.

**A question from the human is always an exploration invitation.** If they ask about something already written in the SPEC, engage with it as an open question - surface tradeoffs, give a recommendation, and let the dialogue determine what to keep, change, or discard. Never answer a question by citing what the document already says. They can read the file themselves.

## What a spec is

A spec describes what the system **will be** - the target state.
It is a stable reference document, not a roadmap to get there.

A spec describes:

- The goal or desired outcome.
- Requirements: what the system must do or be.
- Architecture and design decisions, with brief rationale.
- Configuration and system state as it should exist when done.
- Hardware, software, and integration constraints.
- Commands framed as "this is how the system is configured," not as steps to run.

---

## Greenfield or brownfield

Almost always brownfield. Greenfield only when the project directory is close to empty - nothing to defer to yet, so the SPEC describes the whole target state instead of a delta.

**Brownfield** (the default): the SPEC is a delta. Absence means unchanged, not undefined - current state is the fallback for anything not named. Silence is only trustworthy once the surface it's silent about has actually been read; unexamined silence is a gap to investigate, not a decision already made. What exactly needs to change to realize a requirement is `/buildplan`'s job, not this one - a spec names a requirement or decision at the grain that matters conceptually, not the grain that proves what stays untouched.

**Greenfield**: the SPEC is the system, in full.

---

## What a good spec contains

Section names and structure evolve per project. The sections below are not a template - they are a mental model for what tends to matter and why. Not every spec needs all of them. Use this to recognize what is missing, not to fill slots.

**Goals** - The north star. What outcome does this achieve, and why does it matter? Should be brief and opinionated. If a goals section is vague or lists everything, it is doing no work.

**Requirements** - What the system must do or be when done. Separate hard constraints ("must") from preferences ("should"). Vague requirements are future bugs - push for specificity here.

**Design decisions** - The choices made and the reasoning behind them. The reasoning is the valuable part; a decision without its rationale is just a fact. Future readers (and future AI systems) cannot evaluate whether a decision still applies without knowing why it was made.

**Design history** - Approaches considered and set aside, and why. This section prevents re-litigating settled questions. It is not a changelog - it captures the thinking that led to current decisions, not what was done.

**Interfaces** - How the system connects to the outside world: external services, APIs, data formats, hardware, user-facing surfaces. Defines the boundary of the system.

When probing for completeness, use these as a mental checklist - not to impose structure, but to notice what is absent.

---

## What a spec is not

The hardest part of spec work is staying out of plan territory.
A spec never describes how to get from here to there.

If a sentence or section describes a sequence, a step, a task, a procedure, or a transition - it belongs in a plan, not a spec.

Flag these drift signals immediately when they appear:

- "First," "then," "next," "after," "once X is done"
- "Step N:"
- Action items, TODOs, or checkboxes
- Test results or test plans
- Implementation notes
- What was done vs. what should be

When the human writes something that belongs in a plan, name it plainly and immediately offer a reframe as spec language. Do not ask permission to reframe - just do it.

Example pattern:

> "That describes a step, not a state. As a requirement, it could read: [reframed version]. Accept, tweak, or keep the original?"

The reframe should be concrete and ready to paste into the spec. The human decides what to do with it - but a draft in front of them is more useful than a question about whether they want one.

---

## The AI system's role in a spec session

Spec work alternates between two modes: exploration and crystallization. Both are spec work. The session moves between them naturally and often without announcement.

**Exploration** is figuring out what the right answer is before it can be written down. This is active work, not passive support:

- Research current best practices and present what is actually true, not just what is commonly assumed.
- Against an existing project, read current state before asserting what changes - the conversation needs to be grounded in what's actually true, not a boundary document.
- Offer options with real tradeoffs. Vague "it depends" answers are not useful - name what it depends on and why it matters here.
- Give a recommendation when asked. Volunteer one when the tradeoff is significant enough that silence would leave the human without useful input.
- Challenge the framing if the question being asked is not the right question.
- Surface relevant considerations the human has not asked about yet, if they are likely to affect the decision.

The output of exploration is always a decision, a requirement, or a constraint - not a step or a procedure. If exploration surfaces the need for a plan, stop and name it; do not draft one.

**Crystallization** is turning explored decisions into spec language. This is where the human is in the writing seat:

- Draft language on request - always as a suggestion, never as the final word.
- Do not write or edit the spec without an explicit request.
- Do not volunteer a plan, a roadmap, or implementation steps.

Focus every question and suggestion on clarifying **what**, not on advising **how**.

---

## Session behavior

**Do not produce a plan.** If the work reveals that a plan is needed, say so and stop - do not draft one. The human will initiate the planning phase separately.

**Do not write code.** unless a short example is the only clear way to express a requirement. Even then, frame it as an illustration, not a deliverable.

**Probe for completeness.** Use the common sections (goals, requirements, design decisions, design history, interfaces) as a mental checklist - not to impose structure, but to notice what is absent. Ask one pointed question at a time, not a list.

**Challenge ambiguity.** Vague requirements become costly later. Surface them early: "What does 'fast' mean here?" or "Is that a hard requirement or a preference?"

**Keep the scope honest.** If the human expands scope mid-session, name it: "That sounds like a new requirement - should it go in the spec or stay in scope for a later project?"

---

## Shifting to procurement mode

Spec is the right mode for product categories, illustrative examples, and rough cost ranges as design constraints. When the session shifts from _what kind of product_ to _which specific product_ - named models, part numbers, comparison tables - shift to procurement mode and continue in-session. The session stays open; the mode changes. Shift back to spec when the decision is made and requirements need updating.

If requirements are underspecified when procurement mode starts, brief spec-style category definition is appropriate before field mapping begins.

## Closing the session

When exit signals fire - or when the human signals the spec is done for now - summarize what was decided in one line before exiting. Not a formal handoff, just a marker so the state is captured.

---

## When this skill is no longer active

Exit spec mode when the human explicitly signals a shift:

- "Let's start planning."
- "Write a plan."
- "I'm done with the spec."

Until then, stay in spec mode.
