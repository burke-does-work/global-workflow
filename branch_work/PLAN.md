# PLAN: Roll Out Formatters and Linters Across All Repos

Approved 2026-09-17.

No `SPEC.md` exists for this work, so the target state is carried here, in "Tools and what each owns" and "Config files". Those two sections describe the end state; everything below them is the route.

Bring every repo to the same formatter and linter setup. Consistency is the objective: a repo gets every tool that applies to any file type it holds, and each tool covers every file type it supports.

---

## Tools and what each owns

| Tool              | Applies to                                                                               | Role                                             |
| ----------------- | ---------------------------------------------------------------------------------------- | ------------------------------------------------ |
| Prettier          | `.md`, `.json`, `.jsonc`, `.yaml`, `.yml`, `.css`, `.js`, `.mjs`, `.ts`, `.tsx`, `.html` | Formatter. Rewrites layout                       |
| markdownlint-cli2 | `.md`                                                                                    | Linter. Reports structure a formatter cannot fix |
| ruff              | `.py`                                                                                    | Formatter and linter in one                      |

They do not overlap. Ruff owns Python, Prettier owns everything else it supports, and markdownlint is configured with every Prettier-owned rule disabled so the two cannot fight.

Prettier has no parser for `.toml`, `.sh`, `.lua`, `.conf`, systemd units, or extensionless files. Those stay unformatted, and no tool is added to cover them.

ESLint is deliberately out of scope.

---

## Repos

Under `~/local/documents` unless noted. `temp/aidlc-workflows` is a vendored third-party clone and is excluded.

| Repo                                                    | Prettier covers   | Gets                         |
| ------------------------------------------------------- | ----------------- | ---------------------------- |
| `global_workflows`                                      | md, json, jsonc   | Prettier, markdownlint       |
| `media-dev`                                             | md                | Prettier, markdownlint       |
| `network-infra`                                         | md                | Prettier, markdownlint       |
| `shop-system`                                           | md                | ruff, Prettier, markdownlint |
| `denning_and_outdoorsing/denning_and_outdoorsing_build` | md                | Prettier, markdownlint       |
| `~/local/dotfiles`                                      | md, json, yml     | Prettier, markdownlint       |
| `field-notes-rusty`                                     | ts, mjs, json, md | Prettier, markdownlint       |

Out of scope. Do not touch:

- `ignite-pitch/web`
- `field-notes-site`
- `field-notes`

---

## Decisions

Carried from the draft:

- **Prettier and markdownlint are installed as local dev dependencies, in every repo, including markdown-only ones.** This means `package.json`, a lockfile and `node_modules` in repos that hold nothing but markdown. Accepted deliberately: pinned versions make the check reproducible, and the VS Code Prettier extension resolves the repo-local copy so the editor and the CLI cannot disagree. The alternative, `npx --yes prettier@3` with no `package.json`, was rejected because it pins only the major version and leaves editor and CLI free to differ.
- **`proseWrap: "preserve"`.** Prettier must never reflow prose. This was the original reason Prettier was rejected, and it is the setting that reverses that.
- **markdownlint disables every rule Prettier owns.** `MD004`, `MD009`, `MD012`, `MD032`, `MD035`, `MD047`, `MD060`. Without this the formatter and linter fight: Prettier reformats, the linter complains, the fix gets reformatted back.
- **`prettier.requireConfig` is true in VS Code.** Prettier stays inert in any repo with no `.prettierrc`, so a repo opts in by gaining the file.

Added this round:

- **`global_workflows` is not done.** It holds the config files and is the source to copy them from, but it has no `package.json`, no lockfile and no `node_modules`, and its `.gitignore` does not cover `node_modules/`. Its `## Verify` therefore runs `npx` against whatever version npm resolves that day, which is the alternative the pinning decision rejected. It gets the full procedure like every other repo, and it goes first.
- **Prettier covers every file type it supports, not just markdown.** Prettier began as a JavaScript formatter; TypeScript and JSON are its strongest targets and markdown its weakest. Scoping it to markdown used it only where it is worst. The command is `--check .` everywhere, which also makes the `## Verify` block genuinely identical across repos. The `global_workflows` `README.md` is corrected to match in slice 1.
- **Machine-managed files are excluded.** Any file an application rewrites on its own schedule would otherwise loop: Prettier reformats it, the app rewrites it, the check fails again. Four cases across the repos in scope - `package-lock.json` in `field-notes-rusty` (two of them), `karabiner.json` and `lazy-lock.json` in `dotfiles`, and `cad/tests/fixtures/` in `shop-system`. They go in `.prettierignore`.
- **`shop-system` has no hand-written JSON, so Prettier formats only markdown there.** Of its twenty-one JSON files, twelve are captured Onshape API responses under `cad/tests/fixtures/`, seven are the regenerable API cache in `cad/out/`, one is archived and one is tool-local. Both directories are excluded. `test_generate.py` parses the fixtures rather than comparing bytes, so reformatting would not have failed the suite - the objection is that they record what the API returned, and reformatting diverges them from the next capture for no gain. The only JSON Prettier formats in that repo is the `package.json` the rollout creates.
- **No `--ignore-path` flag, confirmed against the documentation.** Prettier looks for `./.gitignore` and `./.prettierignore` by default, and specifying `--ignore-path` overrides that default rather than adding to it. The original draft's `npx prettier --check . --ignore-path .gitignore` would therefore have disabled `.prettierignore` silently, taking every exclusion in this plan with it. Passing no flag is what keeps both files live. It also ignores `node_modules` and the VCS directories unconditionally, so the `node_modules/` line in `.prettierignore` is belt-and-braces rather than load-bearing.
- **Only the root `.gitignore` is read, not nested ones.** The documentation is specific: the file "in the same directory from which it is run". Every repo in scope keeps its `.gitignore` at the root, so nothing is lost, but a future nested one would be invisible to Prettier.
- **Archive directories are excluded.** `archive/**` in the markdownlint `ignores` and `archive/` in `.prettierignore`, in every repo, whether or not it has an archive directory. Downside accepted: content moved back out of an archive later arrives unformatted and fails the check on the way past. That failure is loud and self-correcting.
- **Every repo gets a `.gitignore`.** `media-dev` and `network-infra` have none. The `global_workflows` file is the starting point, plus `node_modules/`.
- **Every repo gets a `.git-blame-ignore-revs`.** A formatting commit touches every line it reformats, so without this, blame on those lines reports the formatting commit rather than the commit that wrote the content. Uniform across repos rather than only the large ones, so the procedure has no branches.
- **In-flight work is committed before its repo is touched.** `media-dev`, `field-notes-rusty` and `dotfiles` all carry uncommitted changes. A whole-repo reformat landing on top of them would make both diffs unreadable. Committing them is pre-approved; see the affected slices.
- **`dotfiles` is in scope, and so is the VS Code configuration it holds.** Prettier is currently the default formatter only under `[markdown]`, so format-on-save would leave untouched the files the widened CLI now checks. Editor and CLI must widen together or they disagree by construction.
- **Several files in scope are symlinked into live tool configuration.** `~/.claude/CLAUDE.md` points at `global_workflows/AGENTS.md` and `~/.claude/skills` at `global_workflows/skills/`, so slice 1 reformats the instructions the implementing agent is running on. `~/.claude/settings.json` and the active VS Code profile's `settings.json` and `keybindings.json` point into `dotfiles/config/`, so slices 2 and 7 edit live configuration. Nothing here is dangerous - Prettier preserves content and `proseWrap: "preserve"` prevents reflow - but the formatting commits take effect immediately rather than at the next checkout, and a mistake shows up as a broken tool rather than a bad diff.
- **The `ADR.md` entry is corrected in place, not superseded.** `AGENTS.md` and the file's own header both make entries immutable, and this is a deliberate exception rather than a precedent. The rule protects the record of reasoning as it stood; this is a factual error written the same day, in the session the rollout came out of, and it was never acted on. A superseding entry would preserve a wrong statement and add a second one correcting it, leaving the record worse than a one-sentence fix. Slice 9 carries it, and the human review for that slice is where the exception gets confirmed.

---

## Config files

`.prettierrc` and `.markdownlint-cli2.jsonc` are identical in every repo. Copy, do not re-derive.

`.prettierrc`:

```json
{
  "proseWrap": "preserve"
}
```

`.markdownlint-cli2.jsonc` - copy from `global_workflows`, which carries the commented original. Its `ignores` gains `archive/**`, giving:

```json
{
  "globs": ["**/*.md"],
  "ignores": ["scratch/**", "node_modules/**", "archive/**"],
  "config": {
    "MD004": false,
    "MD009": false,
    "MD012": false,
    "MD032": false,
    "MD035": false,
    "MD047": false,
    "MD060": false,
    "MD013": false,
    "MD033": false,
    "MD041": false,
    "MD024": { "siblings_only": true }
  }
}
```

The first block is Prettier's territory. `MD013` is off because the prose does not wrap, `MD033` because inline HTML is used by choice, `MD041` because skill files open with front matter and no H1, and `MD024` is scoped to siblings because repeated subsection names under different parents are deliberate.

`.prettierignore` has a common base, identical everywhere:

```text
archive/
node_modules/
dist/
scratch/
package-lock.json
```

A pattern with no slash matches at any depth, so `package-lock.json` covers the nested one in `field-notes-rusty/deploy/runtime/` as well.

Two repos append to it. `dotfiles` adds the files its applications rewrite:

```text
config/karabiner/karabiner.json
config/nvim/lazy-lock.json
```

`shop-system` adds its captured API fixtures and its regenerable cache:

```text
cad/tests/fixtures/
cad/out/
```

The rule for any future addition: if an application writes the file, it goes in. If a person writes it, it does not. VS Code's own `settings.json` and `keybindings.json` stay formatted, because they are hand-edited and VS Code preserves layout when it writes to them.

`.gitignore`, where the repo has none:

```text
**/.claude/settings.local.json
.DS_Store
node_modules/
scratch/
```

The ruff block for `pyproject.toml` is at `global_workflows/templates/ruff-pyproject.toml`. Copy it whole; it carries comments explaining each rule.

`## Verify` section, identical everywhere, with the ruff lines present only where Python is:

````markdown
## Verify

```bash
npx prettier --check .
npx markdownlint-cli2
uv run ruff check .
uv run ruff format --check .
```

All must pass. If any fails, fix and run again:

```bash
npx prettier --write .
npx markdownlint-cli2 --fix
uv run ruff check --fix .
uv run ruff format .
```

A fresh clone needs `git config blame.ignoreRevsFile .git-blame-ignore-revs` once, so `git blame` skips the bulk formatting commit. Git config is not committed.
````

---

## Per-repo procedure

The same steps in every slice. Slice 1 proves them; later slices repeat them.

- Commit any in-flight work first, alone, where the repo has some.
- Copy `.prettierrc`, `.prettierignore` and `.markdownlint-cli2.jsonc` into the repo root.
- Create `.gitignore` from the template above where none exists; add `node_modules/` to it where one does.
- `npm install --save-dev --save-exact prettier markdownlint-cli2`, creating `package.json` first with `npm init -y` where none exists.
- For `shop-system` only: copy the ruff block into `pyproject.toml` and `uv add --dev ruff`.
- Add the `## Verify` section to the repo `README.md`. Only `global_workflows` has one today, and there it is corrected rather than added. Every repo has a README already, so none needs creating.
- **Commit one, `Build:`.** Config and dependencies only. No formatting changes.
- Run the formatter and linter in fix mode across the repo.
- **Commit two, `Chore:`.** Formatting only, and say so in the subject. Isolating it is what keeps a large diff reviewable.
- Write that commit's hash into `.git-blame-ignore-revs`, and run `git config blame.ignoreRevsFile .git-blame-ignore-revs`.
- **Commit three, `Chore:`.** The blame ignore file alone.
- Confirm every command in that repo's `## Verify` exits zero.

Commit messages follow the template at `~/local/dotfiles/config/git/gitmessage`. No repo in scope carries a `## Commits` section in its README, so the template governs unmodified everywhere.

---

## Slices

Ordered by dependency first - slice 1 establishes the config every later slice copies, slice 2 makes the editor agree with it - then by risk, lowest first. Risk is not the same as file count: slice 6 is the largest reformat and among the safest, because it is all prose.

Each slice's check is the same: **every command in that repo's `## Verify` section exits zero, from a clean tree.** One check, run once, at the end of the slice.

### 1. `global_workflows` - walking skeleton

- `Build: Pin Prettier and markdownlint as local dev dependencies`
- `Chore: Format the repository with Prettier`
- `Chore: Ignore the formatting commit in blame`
- Also in commit one: correct `## Verify` in `README.md` to `--check .`, rewrite the paragraph at `README.md:39` so it does not describe Prettier's scope in markdown-only terms, and add `node_modules/` to `.gitignore`.
- Markdown and one `.jsonc`, and it is the repo the plan lives in, so a mistake is visible immediately. Establishes the config files every later slice copies, and is the first real diff the `proseWrap: "preserve"` setting is tested against.
- `(done)` / reviewed:

### 2. `dotfiles` - editor and CLI parity

- `Chore: Widen Prettier beyond markdown in VS Code`
- `config/Code/matt-profile/settings.json` currently names Prettier as `editor.defaultFormatter` only under `[markdown]`. Widen so the editor formats every type the CLI now checks, and keep `prettier.requireConfig` true so the behaviour stays opt-in per repo.
- That file is symlinked into the active VS Code profile, so the change takes effect live. `config/Code/global/settings.json`, which is symlinked into the default profile, carries no Prettier configuration at all - decide whether to mirror the change there or leave the default profile without Prettier.
- Separate from slice 7 because it is editor configuration, not repo tooling, and it must land before the bulk of the repos are formatted.
- Check: in `global_workflows`, the only repo carrying `.prettierrc` at this point, save a `.md` and a `.json` file and confirm format-on-save fires on both. Confirm Prettier stays inert in a repo without a `.prettierrc`. TypeScript cannot be checked here - no repo in scope has both a `.prettierrc` and a `.ts` file until slice 8, which carries that confirmation.
- `(done)` / reviewed:

### 3. `media-dev`

- `Docs: Add the DAM, pictures and videos specs` - pre-approved, runs first and alone. Covers the three untracked `SPEC_*.md` files, the modified `README.md` and the untracked `archive/`.
- `Build: Add Prettier and markdownlint`
- `Chore: Format the repository with Prettier`
- `Chore: Ignore the formatting commit in blame`
- First repo needing a `.gitignore` created from scratch.
- `(done)` / reviewed:

### 4. `network-infra`

- `Build: Add Prettier and markdownlint`
- `Chore: Format the repository with Prettier`
- `Chore: Ignore the formatting commit in blame`
- Second repo needing a `.gitignore` created. First repo with an `archive/` directory, so it is where the exclusion is observed working.
- `(done)` / reviewed:

### 5. `shop-system`

- `Build: Add ruff, Prettier and markdownlint`
- `Chore: Format the repository with ruff and Prettier`
- `Chore: Ignore the formatting commit in blame`
- Adds ruff to the procedure for the first time. Has a `.gitignore` already, and gains its first declared `## Verify` check, which `DESIGN.md` says a Python project should already have.
- Only 5 Python files, and no JSON that Prettier will touch - all 21 are excluded as fixtures, cache, archive or tool-local. In practice this slice is ruff over 5 files and Prettier over markdown.
- Run `uv run pytest` after the formatting commit as well as the `## Verify` commands. Ruff reformatting the source is a real change to code, even at this size.
- `(done)` / reviewed:

### 6. `denning_and_outdoorsing_build`

- `Build: Add Prettier and markdownlint`
- `Chore: Format the repository with Prettier`
- `Chore: Ignore the formatting commit in blame`
- The large markdown one, and the reason the formatting commit is kept separate. Low risk despite the size.
- This repo has a root `PHILOSOPHY.md`, so read it in full before starting the slice, per the global rules.
- `scratch.md` is excluded by the root `.gitignore` alone, not by `.prettierignore`. Prettier reads that file by default, so it should be untouched; confirm it after the formatting commit.
- `(done)` / reviewed:

### 7. `dotfiles` - repository tooling

- `Chore: Update the codex config` - pre-approved, runs first and alone. Read the diff before writing the subject; the change is uncommitted and its content has not been reviewed here.
- `Build: Add Prettier and markdownlint`
- `Chore: Format the repository with Prettier`
- `Chore: Ignore the formatting commit in blame`
- 13 JSON, 9 markdown and 1 YAML file. The TOML, shell, Lua and extensionless config files are untouched. `karabiner.json` and `lazy-lock.json` are excluded as machine-managed.
- Watch that reformatting `config/Code/**` and `config/claude/**` does not disturb live tool behaviour; these files are read by running applications.
- `(done)` / reviewed:

### 8. `field-notes-rusty`

- `Docs: Update the rusty agent spec` - pre-approved, runs first and alone. Read the diff before writing the subject.
- `Build: Add Prettier and markdownlint`
- `Chore: Format the repository with Prettier`
- `Chore: Ignore the formatting commit in blame`
- Last and riskiest. The only repo where Prettier rewrites code: 43 TypeScript files, 1 `.mjs`, 7 JSON, 8 markdown. No CSS, HTML or YAML. `.gitignore` already covers `node_modules/` and `dist/`; both `package-lock.json` files are excluded by the common base.
- Check `deploy/` before the formatting commit. It is not gitignored and holds a systemd unit and a runtime `package.json`.
- Carries the TypeScript confirmation deferred from slice 2: save a `.ts` file and confirm format-on-save fires and produces no diff the CLI would not.
- Run `npm run test:final` after the formatting commit, not only the `## Verify` block. It chains the build, `scripts/check-code.mjs` and five test suites. Prettier rewriting 43 source files is the one place in this plan where a formatter can break something that still passes a format check. `check-code.mjs` enforces no formatting rules of its own, so there is nothing for Prettier to contradict.
- `(done)` / reviewed:

### 9. `global_workflows` - correct the records

- `Docs: Correct the formatter scope in the design records`
- `ADR.md:11` reads "Prettier formats and markdownlint lints, wherever markdown lives." That is the origin of the markdown-only scoping. Edit the sentence in place so it states that Prettier covers every file type it supports and markdownlint covers markdown. Leave the rest of the entry as written.
- `DESIGN.md` "Setup" names only ruff for a new Python project, which contradicts the consistency objective.
- `README.md:16` points at `docs/design.md`, but the file now sits at the root as `DESIGN.md`.
- Last, because it describes what the rollout established.
- `(done)` / reviewed:

---

## Uncertainty

The ignore-file question that previously sat here is resolved against the documentation and moved into "Decisions". Nothing in the plan now rests on an unverified claim about tool behaviour.

**Slice 8 carries the most.** Reformatting 43 TypeScript files is a different proposition from reformatting prose. The two-commit split and the blame ignore file both matter more here than anywhere else.

**Slice 7 carries a different kind.** Its JSON files configure running applications, so a formatting error there breaks a tool rather than a document. Verify VS Code and Claude Code both still start after that slice.

The earlier caution about `field-notes-rusty` already having Prettier is void. It has no `.prettierrc` and no `prettier` or `eslint` key in `package.json`. Nothing to reconcile.

---

## Human review

After the agent's own verification, per slice:

- The commits are genuinely separate: the `Build:` commit contains no reformatted content, the `Chore:` formatting commit contains no config, and the blame commit contains one file.
- `.prettierrc` and `.markdownlint-cli2.jsonc` are byte-identical to the ones in "Config files", not re-derived. `.prettierignore` matches the common base, plus only the documented additions.
- `## Verify` in that repo's README matches the block above, with ruff lines present only where Python is.
- No file outside that repo changed.

Slice 9 additionally:

- The `ADR.md` edit changed one sentence and nothing else. Read the diff, not the file - an in-place edit to an immutable record is the one change here that the rules would otherwise forbid.

Across the whole plan, at the end:

- All seven repos carry the same config files and the same `## Verify` block.
- `proseWrap: "preserve"` did its job: no prose paragraph was rewrapped anywhere. This is the one failure the per-slice checks cannot catch, because a reflowed paragraph passes every command.
- Format-on-save in VS Code produces no diff in a repo the CLI reports as clean. If it does, editor and CLI have diverged.
- `DESIGN.md` and `ADR.md` agree with what was built.
