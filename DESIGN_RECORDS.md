# Design Records

## 2026-09-06 -- DESIGN_RECORDS replaces DEV_HISTORY

`DEV_HISTORY.md` was documented in `AGENTS.md` from 2026-06-29 and never created once, while `WORK_LOG.md` reached seven repos in the same period. The cause was trigger design. The work log names two observable events -- the human signals completion, or a commit message is drafted -- and the design record said "on completion of a complex task," a judgment always adjudicated downward in the moment. On 2026-08-30 a commit went into `AGENTS.md` to make work log entries unconditional and left the `DEV_HISTORY` block eleven lines above it untouched, which is where the two sections parted.

Chose `DESIGN_RECORDS.md`: one flat file per repo, ADR in spirit, entries immutable, formatted like `WORK_LOG.md` because that is the shape with demonstrated adherence here. Design decisions are defined rather than tested -- significant calls made on the direction of the work -- and the human requests additions. The agent does not ask whether something belongs, with one exception: a retiring `SPEC.md`, where the decisions are already written down and the milestone is observable.

Rejected: folding decision conclusions into a `README.md` section, which cuts against the standard split where reference docs describe what is and decision records describe why. Also rejected: per-module `DECISIONS.md` files, since fewer places to check is worth more than tidier scoping at this volume, with a module prefix on entries covering a future repo split. Also rejected: ADR ceremony -- sequential numbering, status workflow, one file per decision, review gates -- which buys team coordination that has no value solo.

Also rejected: a checkable admission test, drafted four times before being abandoned. Cost of reversal, cost accepted, downstream reach, and a state-versus-action distinction each screened for the wrong property or collapsed in repos whose product is text, where reversing anything is an edit. Significance is not derivable from the artifact; it is a judgment about the direction of the work.

Accepted: the reasoning lives in two files, with only the citation connecting them. And a decision can go unrecorded if the human does not think to add it -- taken because every filter tried admitted or excluded the wrong things, and a wrong entry in an immutable file costs more than a missing one.

Full narrative in `WORK_LOG.md`, 2026-09-06.

---

## 2026-09-06 -- Retiring a SPEC is the human's call

Line 96 of `AGENTS.md` already reserves SPEC alterations to the human, but "alters" does not obviously cover "disposes," and the artifact lifecycle bullet sits in a section otherwise written as agent instructions.

Chose an explicit guard: the human declares retirement, and the AI system carries out the fold only afterward.

Rejected: relying on the existing line 96 rule to cover disposal by implication.

Accepted: one more sentence in a document that otherwise resists per-case detail. Taken because the costs are asymmetric -- an agent folding and disposing a spec unprompted destroys a human-owned document, while the guard costs a line.

Full narrative in `WORK_LOG.md`, 2026-09-06.

---

## 2026-09-06 -- Superseded entries carry no forward pointer

Real ADRs can point a superseded record forward because every record has a stable number. A flat, unnumbered file has no equivalent address, so supersession works only by a new entry naming the old one by date and title.

Chose to leave superseded entries entirely untouched.

Rejected: adding a `Superseded by` line to the original. That would have been the one case where a closed entry is reopened, and the iteration rate here does not justify it.

Accepted: a reader who greps into the middle of the file can land on a dead entry without knowing it. This is safe only because entries are newest-first, which puts a superseding entry above the one it replaces. The two choices are load-bearing together -- flipping to oldest-first would make the forward pointer necessary again.

Full narrative in `WORK_LOG.md`, 2026-09-06.

---

## 2026-08-30 -- Commit style resolves by lookup order

`AGENTS.md` said to "match the commit style of the project" without saying where that style is defined. Working in `dotfiles`, the agent had nowhere to look, sampled `git log`, and picked up a type prefix used once years into the history that was never valid vocabulary.

Chose a lookup order rather than a single source: a `## Commits` section in the repo `README.md` takes precedence where one exists, otherwise the repo's git commit template. Inferring style from `git log` is explicitly prohibited, since history carries strays.

Rejected: giving each docs repo its own commit template. It is conceptually tidier and needs no precedence logic, but `commit.template` lives in `.git/config`, which is neither tracked nor synced -- on any machine where the pointer had not been set, an agent would silently read the global template and use the wrong vocabulary with no signal that anything was wrong. Also rejected: naming the build repo as an exception directly in `AGENTS.md`, which puts per-repo detail into a general working-relationship document where it would accumulate into a list.

Accepted: the precedence depends on a README section existing to discriminate. `denning_and_outdoorsing_build` is currently the only repo carrying one, so the signal is unambiguous today and any future docs repo overrides the same way.

Full narrative in `WORK_LOG.md`, 2026-08-30.

---

## 2026-08-30 -- Work log entries are unconditional

The rule told the agent to ask whether to write an entry, while the interaction rules at the top of the same file say not to ask questions unless prompted. Under that tension the ask kept losing, particularly mid-execution -- missed three times in a row.

Chose to make the entry unconditional: write it, do not ask. Written before the commit message is drafted so it lands in the same commit as the work it describes, and writing it must be flagged in the response.

Rejected: keeping the prompt and carving an exemption into the interaction rules.

Accepted: an unwanted entry can land without being asked for. Mitigated because entries append to the top of the file and are trivial to remove, and because an entry has been wanted every time so far. The unconditional version removes a decision point rather than adding a caveat.

Full narrative in `WORK_LOG.md`, 2026-08-30.

---

## 2026-08-15 -- The work log rule lives in AGENTS.md, not a skill

The logging behavior needed to be passive and automatic, reaching the agent at the right moment without being invoked.

Chose `AGENTS.md`.

Rejected: a skill, which requires explicit invocation per session and so defeats the purpose.

Accepted: the rule loads into every session whether the session produces work worth logging or not.

Full narrative in `dotfiles/WORK_LOG.md`, 2026-08-15.

---

## 2026-08-15 -- Completion is signaled by the human, never asked

The agent writes an entry when a commit message is drafted, or when completion is signaled with words like "done" or "complete". Direct requests also work without prompting.

Rejected: having the agent ask whether something is complete. The signal has to come from the human.

Accepted: work that finishes without either signal produces no entry until one is requested.

Full narrative in `dotfiles/WORK_LOG.md`, 2026-08-15.

---

## 2026-08-15 -- Named WORK_LOG, not AGENT_LOG

The log is a record of the human's thinking, decisions, and actions -- not the agent's. The agent's contributions appear only as context for what the human considered and decided.

Renamed from `AGENT_LOG.md` to remove the agent-centered framing from the concept itself, not just from the instruction.

Rejected: `AGENT_LOG.md`.

Accepted: the filename now says nothing about whose perspective the entry takes, so the framing rule has to be carried explicitly in the `AGENTS.md` instruction.

Full narrative in `dotfiles/WORK_LOG.md`, 2026-08-15.
