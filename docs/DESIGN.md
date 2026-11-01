# DESIGN: Development workflow

The specification for a single-person, spec-driven, agent-assisted workflow built to survive long interruptions.

The behaviour an agent follows is in `AGENTS.md` and the skills. The reasoning behind the choices is in `ADR.md`. This file holds what neither carries: the shape of project assumed, the constraints the workflow must satisfy, and the parts deliberately left out.

---

## Project envelope

The workflow assumes a project of this shape. Outside it, the workflow is a guideline rather than a rule.

- Code, knowledge, or both.
- One repository, one git remote, no CI.
- A code project defaults to Python, managed with `uv`, declared in `pyproject.toml`. It starts as a script, CLI, or library that runs locally, and may later grow a deployed surface or a front end.
- One developer. No reviewer other than the human.
- Secrets in `.env`, gitignored, with `.env.example` committed so the required variables survive a gap.

Where a concern is genuinely project-specific, testing above all, the workflow names a slot and states how to fill it. It does not prescribe the contents.

### Setup

`docs/newproj-checklist.md` lists what a new project needs before the first cycle.

Ruff settings are standardised in `templates/ruff-pyproject.toml` and live per project, because ruff's user-level config is a fallback that any project config shadows entirely, and a cloned repo needs its config to travel with it.

---

## Working files

`SPEC.md` and `PLAN.md` live together in `live_projects/`, ephemeral and disposed at close-out.

A document in draft carries `.draft` in its name: `PLAN.draft.md`, `SPEC.draft.md`. On approval it is renamed and a dated approval line is written inside. Revising an approved document renames it back. The rename tracks scope, not text: if the document now commits to something different it goes back to draft, and if it only reads differently it does not.

Root holds `README.md`, `pyproject.toml`, and the env files - the entry point and what tooling expects to find there by convention. `docs/` holds what's equally permanent but read by a human or an agent following a pointer, not auto-discovered: `ADR.md`, `DESIGN.md`, `WORK_LOG.md`, `newproj-checklist.md`.

---

## Portability

Portable in two directions.

**Operating system.** Linux and macOS, with no per-platform branches in the workflow itself.

- Committed artifacts run unmodified on both. Ad-hoc commands inside a session are unconstrained, since the agent knows its platform and the command dies with the turn.
- Python tooling is unrestricted provided it is declared in `pyproject.toml` and run with `uv run`. That is what makes it portable.
- Committed code avoids platform-only utilities (`pbcopy`, `open`, `xdg-open`) and flags that differ between GNU and BSD (`sed -i`, `date`, `stat`). Python is preferred over shell, as a preference rather than a prohibition.

**Agent.** The workflow does not depend on a feature of one harness.

- A mechanism available in only one agent is out of scope, however well it fits.
- A workaround is acceptable in its place provided it still follows the practice.
- Git-level mechanisms are agent-agnostic by construction, since they fire on the git operation regardless of what invoked it.

---

## Stopping and closing

Implementation stops and reports when any of these fire.

- A slice's check fails twice on that slice. A count rather than a judgment, because an agent in a loop does not recognise the loop.
- The work needs a decision the plan does not contain.
- A slice requires changes the plan does not describe.

The work closes when every slice is done, reviewed and green. Plan size is bounded by review capacity, roughly five to ten slices. Work larger than that means the spec is too big: narrow the spec rather than lengthening the plan or splitting it in two.

---

## Extension to parallel agents

Not built. Recorded because it is a property of the structure rather than a gap in it.

- Each agent takes a non-dependent slice on its own branch, cut from the spec's branch, and at the human's direction squash-merges back as one slice commit.
- The spec's branch and its merge to `main` are unchanged.
- Dependency ordering in the plan identifies which slices can run at once.
- An agent that goes wrong is a branch deleted rather than a commit reverted.

The one piece needing rethinking rather than extending is review. Three agents' work arriving in a single pass is a different proposition from one agent's.

---

## Prior art

Practice has converged on spec, plan, tasks, implement as files rather than session state.

- **GitHub Spec Kit** - MIT licensed, installs into an existing agent rather than replacing it. Workflow is specify, plan, tasks, implement, converge. <https://github.com/github/spec-kit>
- **AWS Kiro** - an IDE built around specs as the unit of work, with hooks that watch file events and flag code drifting from `requirements.md`.
- **`ai-dlc`** - over-engineered for this purpose, but one idea carries: determinism belongs in tools and hooks, knowledge in agents, judgment with humans.

Spec Kit is corroboration rather than supply. Two pieces here were arrived at independently and match it: one place for project rules consulted by every command, which `AGENTS.md` already does, and a dependency-ordered breakdown between plan and implement. The difference is that Spec Kit orders tasks without tying one to a commit, so it has no equivalent of the size test.

One mechanical constraint found while evaluating enforcement: Claude checks file permissions against `Edit(path)` and `Read(path)` only, and rules evaluate deny, then ask, then allow, with allow unable to carve an exception out of a deny. "Block every write except `PLAN.md`" is therefore not expressible as a permission rule.

---

## Out of scope

- CI, or any server-side automation.
- Local file backup. Assumed to exist. Recorded rather than dropped because it drives git process: committing for backup conflates two jobs, and commit frequency then follows anxiety about losing work rather than logical change boundaries.
- Security beyond `.env` hygiene.
- Multi-developer review, approval gates, or branch protection.
- Copying `ai-dlc`. Its useful idea is that the spec is the durable artifact; its ceremony is the part rejected.
- A dedicated branch-position file (`STATE.md`), `ai-dlc`'s other idea. Tried, then dropped - see `ADR.md`.
