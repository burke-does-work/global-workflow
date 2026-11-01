---
name: new-proj
description: New project template to follow to create a new project
disable-model-invocation: true
---

The new project template is the generic template to follow for creating any new project. It applies to both knowledge and code projects. In-prompt directions always take precedent over this template.

## Project docs

### Docs

Create the following docs:

- `README.md`
- `docs/ADR.md`
- `docs/DESIGN.md`
- `docs/WORK_LOG.md`

### Docs content

With the following content:

`docs/ADR.md`:

```markdown
# ADR

**Doc purpose**: Record critical project design decisions.

---
```

`docs/DESIGN.md`:

```markdown
# Design

**Doc purpose**: Documentation of project design details.

---
```

`docs/WORK_LOG.md`:

```markdown
# Work Log

**Doc purpose**: Record project contextual history based AI system in-prompt chat and work. Captures the important concepts that otherwise would be lost when work is finished for the day.

**Content ownership**: Human isn't required to read or approve the AI contribution but will often use as reference.

---
```

````markdown
# README

## Project docs

- `README.md`
- `docs/ADR.md`
- `docs/DESIGN.md`
- `docs/WORK_LOG.md`

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

A fresh clone needs `git config blame.ignoreRevsFile .git-blame-ignore-revs` once, so `git blame` skips the bulk formatting commit. Git config is not committed.

Prettier owns layout in every file type it supports, not markdown alone. markdownlint reports the structural problems a formatter cannot fix, and its config disables every rule Prettier owns so the two do not fight. Neither flag is passed to Prettier for ignores: it reads `.gitignore` and `.prettierignore` by default, and `--ignore-path` would replace that pair rather than extend it. Conventions neither tool covers live in `/style-guide`.

---
````

## Templates

Copy from `global_workflow` repo the contents of `templates/` to the base directory. Files:

- `.markdownlint-cli2.jsonc`
- `.prettierignore`
- `ruff-pyproject.toml`
- `.git-blame-ignore-revs`
- `.gitignore`
