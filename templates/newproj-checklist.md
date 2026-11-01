# New Project Checklist

The generic setup for a new code or knowledge project. Directions in the prompt take precedence, and a project can differ where it has a reason to.

Paths under `templates/` refer to this repo, `global_workflow`.

---

## Decide first

- Project name and a one-line purpose.
- Repo shape - code, knowledge, or both.
- Language, if any.
- Secrets, if any.
- Related repos, which get a cross-reference in `README.md`.

---

## Every project

### Git

- Run `git init` with `main` as the default branch.
- Leave the first commit and any remote to the human.

### Folders

- Create an empty `live_projects/`. Git doesn't track empty folders, so a fresh clone won't have it until it holds a file.

### Tools

Install Prettier and markdownlint locally, pinned to the exact versions in this repo's `package.json`, so a clone and the editor resolve the same build.

```bash
npm init -y
npm install --save-dev --save-exact prettier@X.Y.Z markdownlint-cli2@X.Y.Z
```

Then in `package.json`:

- Set `"private": true`.
- Remove the placeholder `main` and `test` entries.

### Config files

Copy from `templates/`:

- `.markdownlint-cli2.jsonc`
- `.prettierignore`

Create `.prettierrc`:

```json
{
  "proseWrap": "preserve"
}
```

Create `.gitignore`:

```text
.DS_Store
.claude/settings.local.json
.env
node_modules/
scratch/
```

### Docs

Create these by default. A knowledge repo can skip `docs/ADR.md` and `docs/DESIGN.md` until `/closeout` or the work calls for them.

- `README.md`
- `docs/ADR.md`
- `docs/DESIGN.md`
- `docs/WORK_LOG.md`

`README.md`:

````markdown
# Project Name

One-line purpose. Cross-references to related repos.

---

## Project docs

- `README.md`
- `docs/ADR.md` - critical design decisions
- `docs/DESIGN.md` - specification that has outgrown this README
- `docs/WORK_LOG.md` - contextual history

---

## Verify

```bash
npx prettier --check .
npx markdownlint-cli2
```

Both must pass. If either fails, fix and run again:

```bash
npx prettier --write .
npx markdownlint-cli2 --fix
```
````

`docs/ADR.md`:

```markdown
# ADR

**Doc purpose**: Record critical project design decisions.

---
```

`docs/DESIGN.md`:

```markdown
# Design

**Doc purpose**: Document project design details.

---
```

`docs/WORK_LOG.md`:

```markdown
# Work Log

**Doc purpose**: Record project contextual history from AI system chat and work. Captures the important concepts that would otherwise be lost when work finishes for the day.

**Content ownership**: The human doesn't need to read or approve the AI contribution but will often use it as reference.

---
```

---

## Python projects

- Run `uv init`, then `uv add --dev ruff`.
- Paste the block from `templates/ruff-pyproject.toml` into `pyproject.toml`. Don't copy the file itself.
- Add `.venv/` and `__pycache__/` to `.gitignore`.
- Add `uv run ruff check .` to `## Verify` in `README.md`.

---

## Projects with secrets

- Keep secrets in `.env`, which `.gitignore` already covers.
- Commit `.env.example` with the variable names and no values.

---

## Add only when needed

- `archive/` - on the first retired document. Keep it at the root, because the markdownlint ignore only matches a top-level `archive/`. A nested archive needs its own entry in `.markdownlint-cli2.jsonc`.
- `docs/PHILOSOPHY.md` - when durable stances emerge. Agents look for it at this path.
- `## Commits` in `README.md` - when the project departs from the git commit template.
- `.git-blame-ignore-revs` - only after a bulk formatting commit, usually when retrofitting an existing repo. Add the commit's full hash, run `git config blame.ignoreRevsFile .git-blame-ignore-revs`, and note that one-time step under `## Verify`.

---

## Finish

- Run the `## Verify` checks from `README.md`. Both must pass.
- Run `git status` and confirm that `node_modules/` and `.env` don't appear.
