# Global Workflow

The central source of truth for workflow across projects.

Two audiences: AI systems and humans.

Where a project conflicts with anything here, the project decision wins for that project, but this repo remains the default.

Note: This project, `global_workflow`, contains both the files and specifications for global workflow, and the global workflow files used to define and manage this project.

---

## Workflow contents

- `AGENTS.md` - the working relationship and AI behavior contract.
  - Symlinked directly to each tool's global instruction file so it loads at the start of every session.
  - Currently symlinked to `~/.claude/CLAUDE.md` and `~/.codex/AGENTS.md`
  - Covers context, ownership, communication style, command and approval behavior, and the development workflow.
- `skills/` - reference guides that are not auto-loaded.
  - Symlinked to each tool's user skill directory.
- `templates/` - configuration blocks to copy into new projects.

## New projects

Follow `templates/newproj-checklist.md` to set up a new project.

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
