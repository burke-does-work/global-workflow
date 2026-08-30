# Work Log

## 2026-08-30 -- Commit style pointer added; md-guide replaced by style-guide

`AGENTS.md` told the agent to "match the commit style of the project" without saying where that style is defined. Working in `dotfiles`, Claude had nowhere to look, sampled `git log`, and picked up a type prefix that had been used once years into the history and was never valid. The instruction was sound; it just had no destination.

Fixed by stating a lookup order rather than a single source: a `## Commits` section in the repo `README.md` takes precedence where one exists, otherwise the repo's git commit template. Added an explicit prohibition on inferring style from `git log`, since history carries strays.

The precedence exists because `denning_and_outdoorsing_build` deliberately uses no type prefixes -- a docs-as-code convention worth keeping and likely to be reused in future docs repos. A bare pointer to the commit template would have sent agents to exactly the wrong vocabulary there.

Two alternatives were considered and rejected. Giving each docs repo its own commit template is conceptually tidier and would need no precedence logic, but `commit.template` lives in `.git/config`, which is neither tracked nor synced; on any machine where the pointer had not been set, an agent would silently read the global template and use the wrong vocabulary with no signal that anything was wrong. It would also reintroduce duplication inside a single repo. Naming the build repo as an exception directly in `AGENTS.md` was rejected as putting per-repo detail into a general working-relationship document, where it would accumulate into a list. The chosen approach adds no files and needs no per-machine setup. The build repo is currently the only repo with a `## Commits` section, so the discriminator is unambiguous, and any future docs repo overrides the same way.

Two Claude memory files duplicating that README section were deleted in the same pass, leaving the README as the single source. The README was already the more complete of the two, carrying a rule the memories did not record.

The Work Log rule itself was rewritten in the same session, after Claude missed it three times in a row. The cause was a genuine conflict: the rule said to *ask* whether to write an entry, while the newer interaction rule at the top of the file says not to ask questions unless prompted. Under that tension the ask kept losing, particularly mid-execution.

Resolved by making the entry unconditional -- write it, do not ask -- with two additions. The entry is written before the commit message is drafted, so it lands in the same commit as the work it describes rather than trailing it by one, which is what happened throughout this session. And writing it must be flagged in the response, so an unwanted entry is visible and can be removed; entries are appended to the top of the file, so undoing one is trivial. That trade was chosen over keeping the prompt and carving out an exemption in the interaction rules, on the grounds that an entry has been wanted every time so far and the unconditional version removes a decision point instead of adding a caveat.

This commit also carries changes accumulated over earlier sessions. The `md-guide` skill was replaced by `style-guide`, with references updated in `AGENTS.md`, `voice`, and a new Related section in `code-guide`; the style hierarchy moved out of `voice` into `style-guide`. Rules against asking unprompted questions were added to the interaction section and the plan workflow, with a matching line in the `collab` skill. The Work Log section and this file date from the 2026-08-15 session that established `WORK_LOG.md` across five repos.
