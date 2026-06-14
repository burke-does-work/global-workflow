# Global Workflow

The central source of truth for my global rules, voice, and conventions.
Two audiences: AI systems that read these files during a session, and me, as the reference I treat as canonical.

Where a project conflicts with anything here, the project decision wins for that project, but this repo remains the default.

---

## Contents

- `AGENTS.md` - the working relationship and AI behavior contract.
  Auto-loaded in every Claude Code session via a symlink at `~/.claude/AGENTS.md`.
  Covers ownership, communication style, command and approval behavior, the Simple/Complex workflow, and the skills index.
- `skills/` - reference guides that are not auto-loaded.
  An AI system reads the relevant file when a task calls for it.
  See the Skills section in `AGENTS.md` for the full index.
- `README.md` - this index.

---

## Conventions

These files follow the standards they describe.
Prose follows `skills/voice_and_style.md`; Markdown follows `skills/markdown_style_guide.md`.
