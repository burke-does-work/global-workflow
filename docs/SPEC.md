# SPEC: Development Workflow

The target state for a single-person development workflow: spec-driven, agent-assisted, and able to survive long interruptions.

Approved: 2026-09-16

Items marked **Open** are unresolved and carry no decision. Items marked **Decision** are settled, and state the downside accepted. Items marked **Flag** are settled but carry something to build into a skill.

---

## Purpose

- Give one small coding project a workflow that can be adopted immediately.
- Hold the practice in files rather than in memory, so a three-week gap costs nothing.
- Keep the machinery proportionate to one developer. The workflow is subject to its own simplicity test.

---

## Project envelope

The workflow assumes a project of this shape. Anything outside it is a guideline rather than a rule.

- Python, managed with `uv`, declared in `pyproject.toml`.
- One repository, one git remote, no CI.
- Starts as a script, CLI, or library that runs locally. May later grow a deployed surface or a front-end.
- One developer. No reviewer other than the human.
- Secrets in `.env`, gitignored, with `.env.example` committed so the required variables survive a gap.

Where a concern is genuinely project-specific -- testing above all -- the workflow names a slot and states how to fill it. It does not prescribe the contents.

### Setup

What a new project needs before the first cycle.

- `pyproject.toml` via `uv init`, with `ruff` added as a dev dependency.
- The ruff block copied from `global_workflows/templates/ruff-pyproject.toml`. Settings are standardised there; they live per-project because ruff's user-level config is a fallback that any project config shadows entirely, and because a cloned repo needs its config to travel with it.
- `.gitignore`, and `.env` alongside a committed `.env.example` (if there are secrets to manage).
- `branch_work/`, empty.
- `README.md` with a `## Verify` section naming the check. At minimum `uv run ruff check .`

---

## Portability

Portable in two directions: across operating systems, and across agents.

Operating system -- Linux and macOS, with no per-platform branches in the workflow itself.

- Committed artifacts -- tests, hooks, scripts, the verify command -- run unmodified on both platforms. Ad-hoc commands inside a session are unconstrained, since the agent knows its platform and the command dies with the turn.
- Python tooling is unrestricted, provided it is declared in `pyproject.toml` and run with `uv run`. That is what makes it portable: uv resolves the same environment on either platform.
- Committed code avoids platform-only utilities (`pbcopy`, `open`, `xdg-open`) and flags that differ between GNU and BSD (`sed -i`, `date`, `stat`). Python is preferred over shell in the repo because it is easier to keep portable, which is a preference rather than a prohibition.

Agent -- the workflow does not depend on a feature of one harness.

- A mechanism available in only one agent is out of MVP, however well it fits.
- A workaround is acceptable in its place provided it still follows the practice.
- Git-level mechanisms are agent-agnostic by construction, since they fire on the git operation regardless of what invoked it.

---

## Stages

### Key points

The route from intent to merged work.

- Spec. `/spec` produces the spec, the target state. Durable; everything downstream is regenerable from it.
- Plan. `PLAN.md` holds the route -- slices, verification checks, and progress. It lives as long as the branch does, and is disposed at close-out.
- Implement. One plan slice at a time.
- Verify. The project's declared check, plus the review passes below.
- Close out. Reconcile the plan against the spec, then fold the spec into `README.md`, `docs/design_<foo>.md`, and `ADR.md`.
- Merge. Back to `main`.

**Decision**: one `SPEC.md` and one `PLAN.md`. A spec on a branch becomes a plan.

- `SPEC.md` is not a backlog. Scope enters it when it is ready to be worked; anything earlier stays in scratch or a notes file, neither of which this spec defines.
- Two live specs means nothing states what the system is being built toward. Two live plans means nothing states what is being done now.
- All three live together in `branch_work/` -- `branch_work/SPEC.md`, `branch_work/PLAN.md`, `branch_work/STATE.md` -- to keep the project root for permanent files. Not `docs/`, which holds permanent design documents.
- Permanent files stay at root: `README.md`, `ADR.md`, `WORK_LOG.md`, `pyproject.toml`, and the env files.

### Plan

**Decision**: `PLAN.md` carries the plan, not harness plan mode.

- The hinge is multi-session work. Plan mode holds the plan in session state, and branch-scale planning routinely spans a close and reopen of the harness, so the plan would have to be reconstructed each time.
- The second benefit is that nothing depends on the human remembering to request the file before implementation starts. With plan mode the write is a gate held in memory; with a file there is no gate to forget.
- Plan mode is still used for in-session work, and for research and exploration during branch-scale planning. Its `ctrl+g` handoff opens the plan in a text editor, which suits the annotation convention directly.
- Downside accepted: outside plan mode there is no write block. The substitute is the decision below.

Harness context. Plan mode is a permission state, not a different mode of reasoning: it blocks the write tools and adds an instruction to research rather than edit, leaving the model, the context, and the rest of the prompt unchanged. Two things follow. The planning quality attributed to the mode comes from the instruction, so stating the instruction recovers most of what dropping the mode costs. And the block is not absolute -- interactive terminal sessions with bypass permissions available do not enforce it, leaving only the instruction anyway.

**Decision**: the planning constraint is stated in the skill and checked in git. It is not enforced by tooling.

- The planning skill states that planning writes `PLAN.md` and nothing else. This is prose, and prose loses this contest sometimes -- `ai-dlc` ships a hook for precisely this, after a run generated code first and backfilled the plan to match.
- Git supplies the tripwire. Plan from a clean tree, and the changed-files list after a planning session either holds `PLAN.md` alone or it does not. The signal is mechanical and lands in the review surface already in use.
- This is detection, not prevention. Sufficient here because a backfilled plan is only harmful when it goes unnoticed -- the damage is checking work against a document reverse-engineered from that work.
- Rejected: a path-scoped `ask` permission rule, which is harness-specific and needs toggling between stages, reintroducing a gate held in memory. Rejected: a `PreToolUse` hook, harness-specific and the highest build cost of the options.
- Escalation if the tripwire shows the constraint failing: a pre-commit hook refusing any commit that mixes `PLAN.md` with source files. That is the same hook deferred under Verification, doing a second job rather than adding a dependency.

#### Task breakdown

**Decision**: the task breakdown lives in `PLAN.md` as slices. No separate tasks file, no separate command.

- A slice is a vertical cut -- one user-visible capability, end to end, including the check that proves it. Not a horizontal layer.
- Sizing: one verification check per slice. Two checks means two slices. The check is built in the slice, not merely named by it.
- The first slice is the walking skeleton: the thinnest end-to-end path that does something real. It proves the environment sound and the check runnable before anything depends on either.
- Ordered by dependency, then by uncertainty. The plan names the slice it believes most uncertain, so the human reacts to a claim rather than generating one.
- A slice carries two fields: a subject line and a check. The subject is also the commit message, approved at plan time rather than drafted per commit, which is what removes the per-commit message effort.
- Two markers per slice: `(done)` and reviewed.
- Rejected as the sizing rule: no "and" in the commit subject. It is gameable by writing a vague subject. It survives as a secondary smell, not the test.

Two fields, not four. A separate intent line is cut because at this size the subject is the intent -- "Add: config loader reads .env" says it, and a separate line only earns its place when the subject genuinely cannot carry it. Files expected to change is cut because cross-slice edits are expected, so the list would be approximate and routinely violated, and an out-of-scope edit is caught at review anyway. It earns its place later for bounding a parallel agent's context, which is when to add it back.

The test this has to pass is that a plan of six slices is six lines and six checks, plus the ordering and one uncertainty claim -- a page readable in a sitting. Slicing carries the complexity; the plan should not.

Why the slice is cut this way rather than any other: it carries four jobs. One commit, one rollback point, one review unit, and one future parallel-agent assignment. Two fields still satisfy all four. Current practice arrives at the same cut -- a slice can be checked by running it while a layer can only be checked by reading, and reading is the expensive resource. Strict vertical slices are also what let an orchestrator hand a subordinate agent a scoped task with a single verification goal.

#### Drafts and approval

**Decision**: approval is recorded in two places -- the filename, and a dated line inside the file.

- A document in draft carries `.draft` in its name: `PLAN.draft.md`, `SPEC.draft.md`. On approval it is renamed to `PLAN.md` or `SPEC.md`.
- The filename carries it because of the walk-away case. Planning often finishes with no time left for implementation, and weeks later a directory listing has to answer whether the document is still open without anything being opened or read.
- Inside the file, one dated line records the approval. One approval per document, before implementation starts.
- Revising an approved document renames it back to draft. Approving it again replaces the line rather than adding to it.
- The agent performs both the rename and the line, and only on an explicit statement of approval in the prompt. Annotation in the file is never approval.
- Downside accepted: renames churn git history and stale any path reference elsewhere. Git's rename detection covers the first. The second is the price of a signal readable without opening a file.

**Decision**: the rename tracks scope, not text. If the document now commits to something different it goes back to draft; if it only reads differently it does not.

- The agent never renames on its own initiative. Forward only on an explicit statement of approval, backward only when the human says the scope changed.
- Asked to make a change that alters scope in an approved document, the agent says so before editing rather than editing silently.

**Flag**: committing the stage artifact on its own before moving to the next stage reads as general practice rather than a planning-specific trick. It puts stage ordering into history at every transition, at the cost of a commit that was going to happen anyway, and the commit becomes an approval record that is timestamped and attributable in a way a line inside the file cannot be. Consider building it into `/buildplan`.

---

### Planning skill

`/buildplan`. Outline only -- the skill is not written.

- Invoked explicitly and never loaded automatically. It is deliberately constraining, and most planning work does not want it.
- It carries the tool and permission constraints as prose. That is weaker than switching permission mode, and the weakness is accepted for the reasons under the planning constraint decision.

What it covers:

- Entry. Start from a clean tree. Read the spec, the project `README.md`, and the verification section. Research the code before proposing anything.
- Scope. Write `PLAN.draft.md` and nothing else.
- Body. Break the work into slices and state a verification check for each, proven runnable before implementation begins.
- Feedback. Questions and assessment are given when asked for, the way spec sessions already run, rather than volunteered or suppressed by rule.
- Exit. On approval, rename to `PLAN.md` and append the dated approval line.
- Handoff. Implementation starts in a fresh context, reading the plan alone.

---

## Branch lifecycle & git

### Branch

**Decision**: one branch per spec, held across interruption.

- `Build` work goes on a branch. Anything with a spec and a plan behind it belongs there regardless of type.
- `Docs`, `Chore`, `Fix`, and `Refactor` can go directly to `main` when simple and self-contained. A refactor large enough to want a plan is a branch.
- The commit types are the template's, so the split reads off the message rather than needing a separate judgment.
- The branch exists to hold work across interruption, not to contain commits that might be discarded.
- Trunk-only work is rejected. Code is routinely in a state that does not compile, and that state needs somewhere to live other than `main`.
- Lifetime is measured by scope rather than calendar time, however many sessions that takes.
- `main` is merged into the branch at session start, when the tree is clean by definition.
- Push is the final stage. Nothing local is published, so "never rewrite published history" does not bind until push.

### Commit approach

**Decision**: review is one pass over the finished branch. Not mid-work, and not a pull request.

- The human neither reviews nor commits mid-work. Reviewing at the pace an agent commits is too frequent to be useful, and the attention it spends is the scarce input here.
- Review happens once, on the finished item, with exceptions when something fails or goes off track.
- Not through a pull request. A PR is too easy to click through and the interface is not preferred. The diff is read in the human's own editor.
- The human owns conflict resolution against `main` and the final merge.

**Decision**: the agent maintains a state file and commits once per plan slice. The merge preserves those commits.

- The agent commits, once per completed slice. Nothing sub-slice, and no checkpoints.
- Commit boundaries come from the plan, fixed during planning before any code existed. The agent executes a boundary rather than choosing one, which is what makes an unreviewed commit structurally trustworthy.
- `main` is merged into the branch during work, so the final merge fast-forwards. No squash, no rebase, nothing rewritten after review.
- The agent maintains a separate state file, not a section inside a human-owned document. It holds only what git cannot derive.
- Downside accepted: `main` carries agent-written messages reviewed in aggregate rather than one by one. And the staging-as-ledger review mechanism does not survive, because the tree is clean at review time.

The slice boundary carries four things: trustworthy unreviewed commits, the review place-marker, rollback granularity, and the extension to parallel agents.

#### How this was reached

Backup is assumed to exist, set up separately. That removed losing work as a differentiator, leaving review, history, and effort.

Three independent axes -- what the agent commits, merge form, and when review happens. The first two give four combinations.

| Agent commits    | Merge form | Result                                       |
| ---------------- | ---------- | -------------------------------------------- |
| Checkpoints      | Squash     | Coherent. Noise created, noise discarded.    |
| Checkpoints      | Plain      | Noise reaches `main`.                        |
| Meaningful units | Squash     | Real history discarded at the merge.         |
| Meaningful units | Plain      | Fine-grained real history on `main`. Chosen. |

Rejected: squash, which discards commits worth keeping. Sub-slice checkpoints, which buy rollback inside the smallest meaningful unit. No agent commits at all, which is the only option preserving the staging ledger but leaves no rollback point when an unattended run goes wrong. Granular commits reshaped into slices before merge, which is fiddly, easy to skip, and a step held in memory. Human commits plus a work-in-progress commit at each put-down, which adds real work and, with backup in place, protects nothing already unprotected.

`ai-dlc` requires a plain merge at its own top tier so the reviewed commit survives as a direct parent, and refuses squash and rebase there. That matters where an audit trail must prove the link mechanically and the reviewer is not the merger. Here the reviewer, conflict resolver, and merger are the same person in one sitting, so the guarantee is redundant -- except when review and merge fall weeks apart, which the state file covers by recording that review completed.

**Decision**: the branch closes when every slice is done, reviewed, and green.

- Plan size is bounded by review capacity -- what can be read in one sitting, roughly five to ten slices.
- Work larger than that means the spec is too big. Narrow the spec rather than lengthening the plan or splitting it in two.
- Scope is the human's to watch. Nothing enforces it, and nothing needs to.

**Decision**: resume by reading git, then `PLAN.md`, then the state file.

- Git is authoritative on what exists. The state file supplies only intent and what comes next, and the two will disagree, because the file was written before the work continued.
- Half-finished work is reported, not discarded. Git ownership is the human's.
- After an interrupted run, the first action is to update the state file, before any new work. An interrupt gives the agent no chance to write, so the file is stale by however much happened after its last write.
- Plan markers update inside the slice commit, so progress lands with the code rather than depending on a separate write.
- The state file is tracked, so it switches with the branch instead of leaking between branches.

The file itself: separate and agent-owned, because `/collab` and `/spec` bar the agent from writing unprompted and splitting the files keeps content human-triggered while state is agent-maintained. Overwritten rather than appended, which is what separates it from `WORK_LOG.md` -- if an old copy still matters, it is history. No dated entries, one "as of" line, the branch's lifetime.

**Decision**: review runs commit by commit in plan order, marked in `PLAN.md`.

- One commit is one slice, so the commit supplies the unit and the slice's reviewed marker supplies the place. No new mechanism; the old one -- staging ranges from the diff gutter -- does not survive a clean tree.
- This is not the per-commit review that was rejected. What was rejected is interruption at the agent's pace during the work. Reading six commits in one sitting afterwards is a different act.
- Reviewing in commit order reads changes rather than the result, so a file touched by several slices is read more than once. Accepted: reading in plan order tells the story the plan told, and final-state correctness is what the checks are for.

History tidy-up before push was closed by the commit decision, not decided separately. Slice commits are logical when written, there are no checkpoints or put-down commits, and nothing is rewritten after review. Merge commits from merging `main` in reach `main` and are accepted as honest history.

### Extension to parallel agents

Not in MVP. Recorded because it is a property of the structure rather than a gap in it: the arrangement above extends to parallel agents without being redesigned.

- Each agent takes a non-dependent slice on its own branch, cut from the spec's branch, and squash-merges back as one slice commit when it converges.
- The spec's branch and its merge to `main` are unchanged.
- Dependency ordering in the plan is what identifies which slices can run at once.
- An agent that goes wrong is a branch deleted rather than a commit reverted.
- Conceptually this is `ai-dlc`'s two tiers, not exactly: agent-internal churn discarded at the lower merge, the reviewed unit preserved at the upper one.

The one piece that would need rethinking rather than extending is review. Three agents' work arriving in a single pass is a different proposition from one agent's.

---

## Verification

- The project declares its check in one place, named in `README.md` under `## Verify`. This mirrors the existing `## Commits` convention rather than inventing a second lookup.
- The check is defined during spec and plan, not discovered during implementation.
- A check must name its mechanism and its moment, not only its intent. The failure being guarded against is not a failing test; it is a check that was never runnable -- wrong environment, missing tool, or a check that can only pass somewhere the code has not reached yet.
- A check is proven runnable before implementation begins.
- Not every check is automatic. A check the human invokes as a skill is a valid entry in the slot.

Definition happens up front; execution happens at the end. These are one rule, not two.

**Decision**: implementation stops and reports before finishing when any of these fire.

- A slice's check fails twice on that slice. A count, not a judgment, because an agent in a loop does not recognize the loop.
- The work needs a decision the plan does not contain.
- A slice requires changes the plan does not describe.

Stopping means reporting and waiting. Working around a blocker without approval is already barred by `AGENTS.md`. `ai-dlc` reaches the same place by counting refusals per action and halting on the second rather than asking the agent whether it is stuck.

**Decision**: the walking skeleton proves the check runnable, and bootstraps the first one.

- "Proven runnable" means the check ran green on the walking skeleton before any slice depending on it executes. Not asserted, demonstrated.
- That also answers the bootstrap. The project has no suite today, so the first check cannot be written test-first against existing code -- the walking skeleton builds it and proves it by passing.
- If the check cannot be made to run, that is a blocker reported before implementation rather than a problem discovered during it. This is the failure the verification rules exist to catch.

**Decision**: no tooling forces a check to run. It lives in the slice and is run by the agent or the human, and nothing blocks if it is skipped.

- A `Stop` hook was rejected as harness-specific, which the agent portability rule rules out.
- A tracked pre-commit hook via `core.hooksPath` is agent-agnostic and remains the candidate if enforcement is added later. It is deferred because it needs the verify command to exist first and carries its own failure mode against a red suite.
- Downside accepted: a check can silently go un-run, and nothing will say so. The exposure is bounded by the check living in the slice, so an un-run check shows as an unmarked slice rather than disappearing.
- Revisit when the verify command is stable, or if the hook turns out to be cheap to add.

---

## Review passes

**Decision**: two passes, not four. Both run in a fresh session on the branch diff, before the human's own review, and both are skills invoked manually.

- `/reviewsimple`. Is this more than the job needs, and can the human read all of it. Runs without the plan in context, because the plan explains away the complexity that should be questioned.
- `/reviewtech`. An independent view of whether the code does what it claims. Reads the plan, since intent is needed to judge correctness.
- Both read `/code-guide` and `/style-guide`, for opposite reasons. Simplicity is measured against those conventions -- readable means readable to this human, not readable in general. Correctness reads them so it does not flag a deliberate house convention as a defect.
- Cut: automated checks, already run per slice during implementation. The full set runs once at the end as a command, not a pass.
- Cut: coherence, which is what close-out does.
- Findings are reported, not applied. The human decides what to act on.
- A fresh session rather than a subagent: agent-agnostic, and the independence comes from not having written the code.
- The harness ships its own `/simplify` and `/code-review`. Both are declined. These passes are named differently to avoid the collision, and they exist to apply this project's conventions rather than generic ones.

Both are unblocked. Unlike `/buildplan`, which is a skill to stop a constraining instruction leaking into other planning work, these are skills because they run in a fresh session and can inherit nothing from the conversation.

---

## Close-out

Where the thinking held in `SPEC.md` becomes durable, and the step that justifies the spec carrying reasoning at all. The spec captures reasoning that exists nowhere else, so skipping the fold loses the thinking rather than the code -- and the thinking is the part that cannot be reconstructed from the repository.

- Check `PLAN.md` against `SPEC.md`. Deviation during implementation is expected; this is where it is reconciled.
- Update `SPEC.md` to match what was built. This is a merge rather than a separate act, and it is stated explicitly so the step is visible and doubles as a check.
- Fold `SPEC.md` into the durable documents below, then dispose of it. Git retains it. Retiring the spec is the human's call, not the agent's.

The destinations are deliberate rather than redundant.

- `README.md` -- high-level project information. The subdirectory `README.md` is the first destination, layering onto the project `README.md` above it.
- `docs/design_<foo>.md` -- explicit specification that has outgrown a README. Modeling-style work produces enough documented specification to balloon a subdirectory README past usefulness, and splitting it out is the remedy.
- `ADR.md` -- specific design decisions. Renamed from `DESIGN_RECORDS.md` purely for recall: the acronym is memorable where the old name was not. The process is unchanged -- one file, entries at the top, `## yyyy-mm-dd -- Title`, immutable once written. The name borrows the acronym, not the ceremony: no file per decision, no status fields, no template.

**Decision**: `ADR.md` and `docs/design_<foo>.md` stay separate. They have opposite mutability -- a design document describes current state and gets rewritten, while records are immutable once written. Putting immutable entries at the end of a rewritable file invites them being edited, which is the one thing the records rule forbids.

---

## Linting

**Decision**: `ruff` for lint and format, scoped to mechanical rules only.

- One dev dependency through `uv`, one config block. The earlier deferral assumed a setup cost that is not there.
- Mechanical only: unused imports, undefined names, formatting. Anything in `/code-guide` that requires judgment stays there and is checked by `/reviewsimple` and `/reviewtech`.
- It gives the `## Verify` slot something concrete from day one rather than waiting on a test suite.

**Decision**: `markdownlint-cli2` for markdown-only repositories. Not for Python projects.

- `/style-guide` already names markdownlint as its base, so the tool and the written rules are the same thing rather than two implementations that can drift.
- It is a Node package. That is toolchain sprawl in a Python project for the sake of a README, and it is no burden in a repository that is markdown throughout.
- The config is derived from `/style-guide` and validated by running it against files already considered correct, tuning until only real defects remain. Rule-by-rule review is not the method.
- Line length is disabled, since the prose does not wrap. Heading case and the ASCII-over-Unicode preference have no markdownlint equivalent and stay human.
- Building and validating the config belongs in the plan, not here.

---

## Consistency with AGENTS.md

Changes to make at the reconcile. The first three stand regardless of how this spec settles; the last three follow from decisions in it. The lifetime of `PLAN.md` and the close-out destinations also differ between the two files and need aligning.

- The annotation markers are now `#>>:` and `#??:`, and `AGENTS.md` still specifies `#>>` and `#??`.
- A question never implies implementation, at any point in a session. `AGENTS.md` carries the rule but illustrates it only with "why is X done this way?" and "could this be simplified?", which are transparently questions. The form that actually fails is "can you do X?" -- read as a request when it asks exactly what it says, whether the thing is possible. The rule needs that case named, and needs to hold for the whole session rather than reading as plan-stage guidance.
- `WORK_LOG.md` needs its authority stated, not just its content and triggers. It is contextual history: it records what was thought at the time and carries no standing decision. Durable decisions live in `ADR.md` and `README.md`. Without that stated, an agent reads a past entry as a live ruling and imports conclusions that were never settled. Triggered by human only (change from with commits).
- `DESIGN_RECORDS.md` is renamed to `ADR.md`. Same process, memorable name.
- The commit rule needs scoping. `AGENTS.md` bars the agent from running `git commit` unless explicitly asked, and this spec has the agent committing per slice. Resolution: inside an approved plan on a branch, the agent commits per slice; everywhere else the global rule stands unchanged.
- The work log trigger needs rebinding. `AGENTS.md` ties an entry to drafting a commit message, which under agent commits would fire constantly. The entry is written on the human's request only. This is the same trigger-vagueness failure already fixed once.

**Open**: reconcile the two once this spec settles. General practice belongs in `AGENTS.md`; anything specific to this project envelope stays here. A rule stated in both files is a rule that will drift.

---

## Prior art

Practice has converged on spec, plan, tasks, implement as files rather than as session state. Three references worth returning to.

- GitHub Spec Kit -- MIT licensed, reached 1.0 in 2026, installs into an existing agent rather than replacing it, and supports 30-odd agents including Claude Code. Its workflow is specify, plan, tasks, implement, converge. <https://github.com/github/spec-kit>, docs at <https://github.github.com/spec-kit/>.
- AWS Kiro -- an IDE built around specs as the unit of work, with autopilot hooks that watch file events and flag code drifting from `requirements.md` before it reaches review.
- `ai-dlc` -- the AWS framework cloned in `scratch/`. Over-engineered for this purpose, but two ideas carry: determinism belongs in tools and hooks, knowledge in agents, judgment with humans; and a state file holds progress in tracked files rather than session memory.

Spec Kit's value here is corroboration rather than supply. Two pieces of this workflow were arrived at independently and match it: the constitution file, one place for project rules consulted by every command rather than restated per spec, which is what `AGENTS.md` already does; and a dependency-ordered breakdown between plan and implement, which is the slicing under Stages. The difference is that Spec Kit orders tasks without tying one to a commit, so it has no equivalent of the size test.

One mechanical constraint found while evaluating enforcement: Claude checks file permissions against `Edit(path)` and `Read(path)` only, and rules evaluate deny, then ask, then allow, with allow unable to carve an exception out of a deny. "Block every write except `PLAN.md`" is therefore not expressible as a permission rule.

---

## Out of scope

- CI, or any server-side automation.
- Local file backup. Assumed to exist and being set up separately. It is recorded here rather than dropped because it drives git process: committing for backup conflates two jobs, and commit frequency then follows anxiety about losing work rather than logical change boundaries. With backup in place, commit scope returns to the change itself, and the git comparison above turns on review and effort rather than on protecting work.
- Security beyond `.env` hygiene.
- Multi-developer review, approval gates, or branch protection.
- Markdown-only repositories. They can choose trunk or branch and will often stay on trunk, because reading a document is a complete review gate in a way that reading a diff is not. Defining that is separate work. Markdownlint will apply equally here but markdown repos aren't of concern right now.
- Copying `ai-dlc`. Its useful idea is that the spec is the durable artifact; its ceremony is the part being rejected.
