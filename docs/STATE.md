# STATE

As of: 2026-09-17

Agent-maintained. Overwritten, never appended - only the current copy matters. Thought history belongs in `WORK_LOG.md`, decisions in `ADR.md`. Disposed at close-out.

---

## Stage

Implementation complete and committed. Waiting at the sign-off gate before close-out.

A file-by-file review of `9f63af2` is running ahead of sign-off. Progress is tracked under `## Review order`.

Branch: `docs/git-workflow`. `docs/SPEC.md` and `docs/PLAN.md` are both approved, dated 2026-09-16.

---

## Where work stopped

All eight slices in `docs/PLAN.md` are marked `(done)` and landed in `9f63af2`, which is pushed. Both checks pass: `npx prettier --check` and `npx markdownlint-cli2` exit zero.

Everything since is uncommitted. The `/style-guide` review changed four sections and trimmed `/collab`:

- `## Punctuation and symbols` replaces `## Typographic substitutions`. ASCII rule, a dash system, and exceptions split by whether an ASCII form exists or is wrong.
- `## Tables` reworked. Fragment cells, a six-column cap, detail moved above or below the table.
- `### Task lists` added. Status is stated in words.
- The Markdown dialect line moved into `## Style hierarchy`.
- `/collab` lost its list, checklist and status-marker rules, now held globally.

The `AGENTS.md` review is in progress and has landed:

- A conciseness pass. Cuts, merges and tightened sentences across the file.
- `## Skills` deleted entirely, twelve entries. Both tools read skill front matter directly, so the list was a second copy that had already drifted once.
- `## Git workflow` and `## File annotations` lifted from subsections to top-level sections.
- The emoji rule removed from both files, covered by the ASCII rule.
- A dash sweep of `AGENTS.md` and `WORK_LOG.md`, and the entry heading format at `AGENTS.md:161`.
- `## Context and philosophy` added: two repository shapes, idea dispersal, skills as partial, the cross-domain framing, and the `PHILOSOPHY.md` lookup moved out of `/collab` so it applies in every mode.
- `## Modes` cut entirely. Complexity is signalled by the human, never inferred.
- `## Development workflow` rewritten around the full arc, with compression stated as a property rather than an exception, and two opposite failures named.
- `## Completion` gained the reconcile step present at `SPEC.md:71` and absent here, and now admits archiving alongside disposal.
- `### Implement and test` became `### Implement and verify`, pointed at the `## Verify` convention.
- `README.md` references to the deleted `## Skills` section fixed.
- `### Plan` cut to two sentences. Five of its six bullets duplicated `/buildplan` or the Communication style rules, and one contradicted `/buildplan`'s two-field slice rule.
- The `PHILOSOPHY.md` read consolidated into `AGENTS.md` from `/collab`, `/spec` and `/procurement`, with precedence broadened to cover skills as well as `AGENTS.md`. `/procurement` no longer announces when the file is absent.
- Vocabulary aligned on "check" throughout, matching `SPEC.md`, `PLAN.md` and `/buildplan`. No occurrence of "test" or "acceptance criteria" remains.

Deviations are tracked in `docs/PLAN.md`, in the `/style-guide` and `AGENTS.md` slices and at `## Close-out`. A commit message was drafted early in the review and is now out of date. `skills/` is symlinked to `~/.claude/skills` and `~/.agents/skills`; `AGENTS.md` is symlinked to `~/.claude/CLAUDE.md` and `~/.codex/AGENTS.md`. All four are directory or file symlinks resolving by path. All edits are live globally.

---

## Review order

Ordered by dependency, instrument first. Marked as the human clears each.

- `.markdownlint-cli2.jsonc` (pass)
- `.prettierrc` (pass)
- `README.md` (pass)
- `AGENTS.md` (pass)
- `skills/procurement` and `skills/dashu` (pass)
- `skills/code-guide` (pass)
- `skills/spec` (pass)
- `templates/ruff-pyproject.toml` (pass)
- `skills/buildplan` (pass)
- `skills/reviewsimple`
- `skills/reviewtech`

`/style-guide` was reviewed and reworked before this list was set. (pass)

Out of scope by decision: `docs/SPEC.md` and `docs/PLAN.md`, both approved; `docs/STATE.md`; `WORK_LOG.md`; the `ADR.md` rename; and the two deleted workflow files.

---

## Next

- Continue the review at `AGENTS.md`.
- Human reviews the branch and gives implementation sign-off. The plan states that slices passing their checks is not approval.
- On sign-off, close-out runs: spec folds into the skills, `AGENTS.md`, `design.md` and `ADR.md`, then `docs/SPEC.md` and `docs/PLAN.md` are disposed.

---

## Outstanding

- `denning_and_outdoorsing_build/README.md` still carries a local override of the completion arc, and it cites the superseded `DEV_HISTORY.md`. `AGENTS.md` now admits archiving, so that override can be retired or corrected. Different repo.
- Codex skill discovery is unconfirmed since 2026-09-13. `WORK_LOG.md:149` verified all eight skills loading through `~/.agents/skills`; current Codex documentation names `~/.codex/skills`. Whether both paths are read is unknown, and only a live Codex session running `/skills` settles it.

Files still carrying `--`, since the sweep covered only `AGENTS.md` and `WORK_LOG.md`: `README.md`, `docs/PLAN.md`, `docs/SPEC.md`, `templates/ruff-pyproject.toml`, and the skills `buildplan`, `reviewsimple`, `reviewtech`, `learn-dev` and `walkplan`.

Carried from before the review:

- Four items were held at the `AGENTS.md` reconcile and still need sorting at close-out: `PLAN.md` lifetime, the slice definition, the `.draft` convention, and the stop triggers.
- The spec rule excluding markdownlint from Python projects is wrong and contradicts the consistency objective set later. `shop-system` holds nine markdown files. Correct it at close-out or record the correction in `ADR.md`.
- Every skill front matter `description` carries an em dash, which the new punctuation rule forbids. Deferred unless the file is being edited anyway.
- The degree sign and prime-mark rulings sit in `## Units of measurement` rather than with the ASCII rule. The same rule in two places.
- The commit template in `~/local/dotfiles` uses `--` as its bullet separator, which the new dash rule no longer permits. Different repo.
- `docs/PLAN.md` sets two mid-work gates, after the markdownlint config and before the `/style-guide` trim. No approval is recorded at either.
- `scratch/PLAN.draft.md` holds a separate, unapproved rollout plan for the other repos. It is not part of this work and runs on its own.
- The three new skills have not been invoked for real. Their checks verify scope, not judgment. That shows on first use.
