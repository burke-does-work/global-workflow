# STATE: Formatter and Linter Rollout

Progress against `PLAN.md`. Written so a fresh session can pick the work up without the conversation that produced it.

Last updated 2026-09-17, after slice 9.

---

## Where the work stands

All nine slices complete. Every repo passes its `## Verify` commands from a clean tree.

| Slice | Repo                                     | State                              |
| ----- | ---------------------------------------- | ---------------------------------- |
| 1     | `global_workflows`                       | Complete                           |
| 2     | `dotfiles` - editor and CLI parity       | Complete, editor check outstanding |
| 3     | `media-dev`                              | Complete                           |
| 4     | `network-infra`                          | Complete                           |
| 5     | `shop-system`                            | Complete, 91 tests passing         |
| 6     | `denning_and_outdoorsing_build`          | Complete                           |
| 7     | `dotfiles` - repository tooling          | Complete                           |
| 8     | `field-notes-rusty`                      | Complete, 33 tests passing         |
| 9     | `global_workflows` - correct the records | Complete                           |

Nothing has been pushed. Every repo touched is on a branch named `workflow-rollout`, created off `main`.

---

## Resume here

Nothing in the plan is outstanding. What remains is the human review listed at the end of `PLAN.md`, then close-out.

The one check an agent cannot run: **format-on-save in VS Code**. Open a markdown file in `global_workflows` and a `.ts` file in `field-notes-rusty`, save each, and confirm both reformat. Both repos now carry a `.prettierrc`, so `requireConfig` is satisfied in each.

---

## Local state not carried by git

Run once per clone, in every repo:

```bash
git config blame.ignoreRevsFile .git-blame-ignore-revs
```

Done in all seven on this machine. It does not travel with a clone.

---

## Carried forward

- **The `smol-toml` advisory** rides along with `markdownlint-cli2` in every repo. High severity, denial of service via malformed TOML, dev dependency only. Accepted deliberately; the only remedy npm offers is a downgrade.
- **`compute/maodou-pi/design_containers.md` in `network-infra`** uses Unicode box-drawing characters, which the style guide's ASCII rule would flag. No tool checks it.
- **Em-dashes in `dotfiles`** README and VS Code settings comments, same situation.
- **`scratch.md` in `denning_and_outdoorsing_build`** carries unreverted edits from the tool-order mistake. Gitignored and disposable.

---

## Verified during implementation, worth not re-deriving

- Prettier reads `./.gitignore` and `./.prettierignore` by default, and `--ignore-path` replaces that pair rather than extending it. No repo passes the flag.
- Only the **root** `.gitignore` is read. Nested ones are invisible, which is why `.pytest_cache/` needed an explicit exclusion in `shop-system`.
- `MD036` fires only on a standalone bold line with **no terminal punctuation**. A bold sentence ending in `.`, `?`, `:`, `;`, `!` or `,` passes.
- **Run any blank-line or parse repair before `markdownlint --fix`.** Its `MD026` fix strips trailing punctuation from anything it reads as a heading, and both checks pass afterwards without complaint. Scan the diff for lost punctuation rather than trusting the checks.
- Prettier rewrites `.markdownlint-cli2.jsonc` to add trailing commas. Valid JSONC, and markdownlint-cli2 parses it without complaint.
- `proseWrap: "preserve"` held in every repo. No prose paragraph was rewrapped anywhere.
- **Physical line counts are formatter-owned.** Any size limit measured against hand-wrapped source has to be recalibrated once a formatter lands; the same modules grew about 1.45x with no code added.
