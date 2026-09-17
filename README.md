# Global Workflow

The central source of truth for my global rules, voice, and conventions.
Two audiences: AI systems that read these files during a session, and me, as the reference I treat as canonical.

Where a project conflicts with anything here, the project decision wins for that project, but this repo remains the default.

---

## Contents

- `AGENTS.md` - the working relationship and AI behavior contract.
  Symlinked directly to each tool's global instruction file, currently `~/.claude/CLAUDE.md` and `~/.codex/AGENTS.md`, so it loads at the start of every session.
  Covers context and philosophy, ownership, communication style, command and approval behavior, the development workflow, and close-out.
- `ADR.md` - key design decisions and the reasoning that forced them. Immutable entries, newest at the top.
- `docs/design.md` - the development workflow specification: the project shape it assumes, the portability constraints it satisfies, and what is deliberately out of scope.
- `skills/` - reference guides that are not auto-loaded.
  Symlinked to each tool's user skill directory, currently `~/.claude/skills` and `~/.agents/skills`.
  An AI system reads the relevant file when a task calls for it.
- `templates/` - configuration blocks to copy into new projects.
- `README.md` - this index.

---

## Verify

```bash
npx prettier --check "**/*.md" --ignore-path .gitignore
npx markdownlint-cli2
```

Both must pass. If either fails, fix and run again:

```bash
npx prettier --write "**/*.md" --ignore-path .gitignore
npx markdownlint-cli2 --fix
```

Prettier owns layout - table alignment, list markers, blank lines, horizontal rules, trailing newline, whitespace. markdownlint reports the structural problems a formatter cannot fix, and its config disables every rule Prettier owns so the two do not fight. Conventions neither tool covers live in `/style-guide`.
