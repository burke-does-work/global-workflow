# PLAN: Implement the Development Workflow SPEC

Approved: 2026-09-16

Route from the approved `docs/SPEC.md` to a working setup in this repo.

This is a markdown repository, so the slice-per-commit rule does not apply. Slices are kept because they carry the verification checks; commits stay at your discretion.

---

## Scope

In scope: the three skills the spec specifies, the markdown check, and the `AGENTS.md` and `README.md` updates that follow.

Out of scope: anything the spec defers to a Python project -- `branch_work/`, `pyproject.toml`, the ruff block, `.env`. The ruff template already exists and is done.

---

## Gates

Review sits at the end of the work, per the spec. Three points are exceptions, and each is named at the slice it belongs to.

- After the markdownlint config. It is the instrument that measures every slice after it, so reviewing it last would mean seven checks ran against an unreviewed standard.
- After the coverage report and before `/style-guide` is trimmed. The trim edits a live global file on the strength of the report.
- Before close-out. Implementation sign-off is required and the fold does not start without it.

Work stops at each gate and reports. It does not continue on the assumption of approval.

---

## Slices

Ordered by dependency. The check comes first because everything after it is verified by it.

### Add markdownlint-cli2 with a config derived from /style-guide (done)

**Deviation.** This slice said no formatter would be added and that Prettier was rejected. Prettier was adopted instead, with `proseWrap: "preserve"` — the setting that removes the original objection, which was prose reflowing. The cause: `MD060` in aligned mode detects misalignment and has no fixer, so enforcing the table style required a formatter. Writing one by hand was rebuilding Prettier badly. markdownlint now disables every rule Prettier owns and reports structure only.

The walking skeleton. Proves the check runs before anything depends on it.

Source of truth is `skills/style-guide/SKILL.md`. Every setting traces to a line in it.

Settings the guide states directly:

- `MD004` ul-style `dash`. The guide mandates `-` and forbids `*`.
- `MD035` hr-style `---`.
- `MD013` line-length off. The guide sets no line length.
- `MD001`, `MD032`, `MD036`, `MD047` on -- heading increment, blank lines around lists, no bold-as-heading, trailing newline.

Everything else stays at default. Where a default fires on content already considered correct and the guide is silent, existing practice wins: disable the rule and note why. That tie-break is needed because "only set what the guide states" and "leave the rest at default" otherwise collide during tuning.

Three items from the parallel evaluation that the repo contradicts or complicates:

- `MD033` no-inline-html off. Not because HTML is already present -- the repo's only HTML is comments, which `MD033` does not flag, and backticked `<foo>` placeholders that parse as code spans. Off because inline HTML is used occasionally by choice and `/style-guide` is silent on it, so the default would flag it the next time.
- `MD041` first-line-heading off. Skills open with front matter and then prose, with no H1 by design, and their front matter uses `name` rather than `title` so the `front_matter_title` default does not satisfy the rule either. Turning it off changes no files. `MD025` stays on -- it fires on multiple H1s, not on zero, so skills do not trip it.
- `scratch/` holds a gitignored clone of thousands of markdown files including a 628 KB changelog. The glob must exclude it or the check is unusable.

Tune against `AGENTS.md`, `README.md`, `docs/SPEC.md`, and the skills. Anything flagged there is either a real defect or a rule that fights the house style.

**Check**: `npx markdownlint-cli2` exits clean across the repo with `scratch/` excluded, and every suppression traces to a stated `/style-guide` position or a noted conflict with existing practice.

**Gate**: stop here. Present the config and the list of suppressions with the reason for each. Each suppression is a judgment that a rule does not apply to these conventions, and that call is the human's. Nothing proceeds until it is approved.

### Produce the style-guide coverage report (done)

Walk `skills/style-guide/SKILL.md` top to bottom and map each stated rule to a markdownlint rule ID, noting auto-fixability, or to "not covered."

It has two consumers rather than being documentation for its own sake. It tells `/reviewsimple` and `/reviewtech`, which both read `/style-guide`, which conventions a tool already enforces and which are theirs to check. And it drives the next slice, which removes the enforced ones from the guide.

Gaps to confirm rather than rediscover: heading capitalization, ASCII typographic substitutions, no numbering in steps or headings, the presence of `---` between `##` sections as opposed to its style, H1 matching the front matter title, and the units and keybindings sections.

One to resolve: the guide requires tight lists, with no blank line between a parent item and its nested sub-items. I could not find a markdownlint rule that enforces this either. If none exists, it goes in the report as not covered and stays human.

**Check**: every section of the style guide appears in the table with a rule ID or an explicit "not covered."

**Gate**: stop here. The next slice edits `/style-guide`, a live global file, on the strength of this table. Present it and wait.

### Trim /style-guide to what the linter cannot enforce (done)

Anything markdownlint enforces comes out of the guide, the same split `/code-guide` made for ruff. A rule stated in both places is a rule that drifts, and the guide should hold only what needs a human.

Expected to leave, subject to the coverage report: dash bullets, blank lines around list blocks, no bold-as-heading, heading increments, trailing newline.

Expected to stay: heading capitalization, ASCII typographic substitutions, no numbering in steps or headings, `---` placement between `##` sections, tight lists, units of measurement, keybindings.

The guide keeps a line naming markdownlint as the enforcing tool without restating its rules, so changing the config never requires editing the guide. `/code-guide` already carries the equivalent sentence for ruff.

**Check**: nothing in `/style-guide` duplicates a rule the config enables, and the markdown check still exits clean.

### Add a Verify section to README.md (done)

- Names the markdown check as this repo's one command, following the `## Verify` convention the spec sets.

**Check**: the command named in `## Verify` runs and exits clean.

### Write /buildplan (done)

- Explicitly invoked, never auto-loaded. Constraining by design.
- Covers entry, scope, body, feedback, exit, and handoff as outlined in the spec.
- Carries the write constraint as prose, with the git tripwire as the backstop.

**Check**: invoked in a scratch repository against a trivial spec, it produces `PLAN.draft.md` and leaves the changed-files list holding nothing else.

### Write /reviewsimple (done)

- Runs in a fresh session on a branch diff, without the plan in context.
- Reads `/code-guide` and `/style-guide`; measures against those conventions rather than generic ones.
- Reports findings, applies nothing.

**Check**: run against a deliberately overbuilt sample, it names the overbuild and edits no files.

### Write /reviewtech (done)

- Runs in a fresh session on a branch diff, with the plan in context.
- Reads `/code-guide` and `/style-guide` so house conventions are not reported as defects.
- Reports findings, applies nothing.

**Check**: run against a sample carrying a known defect, it finds that defect and edits no files.

### Update the AGENTS.md skills list (done)

- Adds the three new skills with their triggers.
- Corrects the `/walkplan` entry, which currently reads as a coding mode and is used for physical builds.

**Check**: all skills present with one-line triggers; the markdown check still exits clean.

---

## Uncertainty

The markdownlint config is the most uncertain slice, and the reason it is first. What remains unknown is how many defaults fire on content already considered correct. The tie-break above handles each case, but the volume decides whether this is five minutes or an afternoon -- and if it is the latter, that is a signal to narrow the rule set rather than keep tuning.

Second, and worth flagging before the work starts: skill behavior is hard to check mechanically. The checks above are the honest version -- each invokes the skill and inspects what it wrote -- but they verify that a skill runs and stays in scope, not that its judgment is good. That part is yours on first real use.

---

## Close-out

**Gate**: implementation sign-off is required before any of this begins. The slices landing and their checks passing is not approval. Report completion and wait.

After sign-off, the spec folds and is disposed:

- Behavior an agent follows in a specific mode goes to the skills.
- Behavior that applies everywhere goes to `AGENTS.md`. Most of this was done at the reconcile.
- Everything else goes to `design.md`.
- The key decisions behind `design.md` go to `ADR.md`.

Items deliberately held at the reconcile and still to be sorted here: `PLAN.md` lifetime, the slice definition, the `.draft` convention, and the stop triggers.

---

## Settled during planning

The `.draft` rename is human-directed for the spec. `/spec` bars the agent from writing to a spec unprompted and a rename is a write, so the agent renames on an explicit instruction rather than on its own reading of an approval. `/buildplan` follows the same rule for plans.
