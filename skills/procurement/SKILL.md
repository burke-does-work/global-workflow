---
name: procurement
description: Behavioral guide for procurement sessions - technical parts, tools, and general goods. Load when the human is comparing products, evaluating a purchase, or building a decision that warrants a written writeup.
---

This skill governs how the AI system participates in procurement sessions.
Read it before responding to any procurement request.

## On load: locate domain context, do not front-load

Do not read the domain reference files end-to-end at session start. Reading them wholesale wastes attention and drifts the session.

Instead, ask the human once at the top: **"Is there a specific domain or existing project this ties into?"** Accept any answer, including "not sure" or no answer. Do not present a menu of domains - the human names one or does not.

If a domain is named or clearly implied:

- Use `grep` (or the equivalent) against the relevant `*<domain>*.md` and any matching `live_projects/*.md` file for the specific product category, ecosystem term, or part family (e.g. `grep -i "blade\|kerf" workshop/workshop_ref.md`, `grep -i "makita\|xgt" workshop/workshop_ref.md`).
- Pull only the sections that hit. Read those sections, not the whole file.
- If nothing hits, proceed without prior context and note that.

If no domain is named:

- Proceed on the current prompt.
- If an ecosystem term surfaces during the session (a battery platform, a connector family, a thread class, a rail size), grep at that point.

Prior context matters because ecosystem is central: a product that requires a new connector, battery, hose diameter, or thread family is a real cost even when the sticker price is low. But context is pulled on demand, not preloaded.

**How to interpret what is found:** these files carry decisions already made. Do not relitigate them by default. Cite the prior decision when it constrains the current one, and flag it plainly if new information suggests the prior call was wrong.

---

## What procurement is

A procurement writeup documents a buying decision.
It is a durable reference document, not a shopping list.

The output describes:

- The take - the decision, one line at the top.
- The comparison - options actually considered, with model numbers and the specs that matter.
- The rejected paths - what was ruled out and why.
- The ecosystem link - what family the product joins.
- The source thread - manufacturer datasheets, forums, video, and independent teardowns that informed the call.

### Interactive by default

Procurement sessions are interactive. Do not write to a file during the working phase unless the human names one in the opening prompt. Work in-conversation - draft the comparison table, surface the tradeoffs, propose a take, iterate. The file write is a separate closing step (see Closing the session).

If the human opens with an explicit destination ("add this to `workshop_ref.md`", "put it in `live_projects/blades.md`"), skip the closing prompt and write there directly.

Where things live when a file is written:

- Scratch capture goes in `scratch.md`.
- Active comparison goes in `live_projects/<project>.md`.
- Settled decision rolls into the domain `*<domain>*.md`.

---

## What a good writeup contains

Section names and structure evolve per purchase. The sections below are a mental model of what tends to matter and why. Not every writeup needs all of them. Use this to notice what is missing, not to fill slots.

**Take (or TLDR)** - the decision as one line at the top. If a person reads only this line, they get the answer. A vague take ("looked at several options") is doing no work.

**Use case** - one sentence naming the constraint. Load, size, environment, frequency, ecosystem to match. Vague use cases produce vague writeups.

**Comparison table** - the options considered, side by side, with the spec that decides the tie. Right-align numeric columns.

**Options considered** - one short block per option with a manufacturer link. Retailer links are secondary.

**Rejected / Ruled out** - paths said no to, with the reason. Not padding - it prevents relitigating and captures thinking the comparison table cannot hold.

**Held** - viable but deferred. Distinct from rejected: the option may come back later.

**Ecosystem fit** - the connector, battery, thread class, hose diameter, rail system, or extrusion series this joins. Skip if not relevant, name explicitly if it is.

**Certification and material spec** - grade markings, material class, UL listing, MBS/WLL rating, IP rating - for anything structural, exterior, or safety-relevant. Generic hardware rarely carries real grade markings even when it claims to. Treat certification as part of the identifier. When no mill cert or grade marking exists, name it as a limitation, not a footnote.

When probing for completeness, use these as a checklist. Ask one pointed question at a time.

---

## What procurement is not

Procurement does not produce an installation procedure, a build plan, or a project schedule.
It ends with a buying decision and its rationale, not a sequence of steps.

Flag these drift signals when they appear:

- "Step N:" or numbered installation steps
- "After buying, do X, then Y"
- Test plans or testing procedures tied to the purchase
- Action items or TODOs

Naming these does not mean cutting the content - it means moving it. Install steps belong in a project file or a plan, not in the procurement writeup that decides which product to buy.

Example pattern:

> "That is install sequencing, not procurement. As a rejected-path note, it could read: [reframed version]. Move it to the project file, or keep it and move on?"

---

## Sources

### Prefer

- **Manufacturer product pages and datasheets.** Anything with a PN and a downloadable PDF. Makita, Festool, 8020, McMaster-Carr, Digi-Key, Mouser, Amphenol, Rottefella, product manuals.
- **Dedicated forums.** Signal-to-noise is higher than broad subreddits. Festool Owners Group (FOG), TelemarkTalk, Tacoma World, Mike Holt Forums, r/Makita.
- **Named YouTube channels with a repeat point of view.** Peter Millard for track saws and MFT work, Project Farm for teardowns, Stumpy Nubs for woodworking depth. A channel that keeps coming back with sharper insight beats a video that ranks highly in search.
- **Independent teardowns and marine or industrial test articles.** Especially for exterior fasteners, marine finishes, and structural cordage.

### Use, but weight down

- **Amazon reviews.** Star ratings are gameable. Read only 1-star and 3-star reviews for real failure modes. Ignore aggregate stars.
- **Broad subreddits.** Useful for spotting names to research. The median comment reflects median use, which is not the right buy for a specific case.
- **Blog roundups.** Almost always affiliate content. Useful only to find the shortlist.

### Skepticism principles

- If four sources cite the same brand, check whether they cite the same review. Consensus can be one voice repeated.
- Popularity in a hobbyist community is not the same as correctness. Hobbyist communities converge on identity purchases.
- Assume glowing single-video reviews are compromised. Sponsored content is not always disclosed.
- The middle of the bell curve is not the answer. It is an input, not a finish line. Independent assessment against the actual use case beats consensus.

---

## The AI system's role in a procurement session

A procurement session alternates between two modes: field mapping and decision framing. Both are procurement work. The session moves between them without announcement.

**Field mapping** is naming who the serious players are and what the specs say. This is active work:

- Identify the manufacturers in the space - boutique premium, professional-tier, prosumer, generic white-label.
- Read the spec sheet, not the marketing.
- Surface the two or three numbers that actually decide the choice (kerf, hook angle, ampacity, load rating, thread class, IP rating, weight per meter).
- Cross-check forums and video for use-case failures and long-term durability, not for consensus.

**Decision framing** is turning the field into a written call:

- Draft the take.
- Build the comparison table.
- Name the rejected paths explicitly, with reason.
- Flag the ecosystem link.

Give a recommendation when asked. Volunteer one when the tradeoff is significant enough that silence would leave the human without useful input. Challenge the framing if the question being asked is not the right question.

---

## Session behavior

Format and procedure rules. Principles (quality-for-money, ecosystem cost, boutique vs. function) live in `PHILOSOPHY.md` and are the authority when in doubt.

**Include the manufacturer part number.** A recommendation without a PN is a lead, not a conclusion. `B-57342`, not "the Makita 6.5-inch blade." `40-1984`, not "the slide-in t-nut." For hardware, include grade and material as part of the identifier: "M6 × 25 mm SHCS, 12.9 zinc-plated", not "M6 bolt". Do not rely on memory for specific part numbers.

**Use tables for anything with 3+ options or 2+ comparable axes.** Right-align numeric columns. Baseline structure:

| Name | Part #  | Price | Key spec | Comment |
| ---- | ------- | ----: | -------- | ------- |
| ...  | XX-1234 |   $12 | ...      | ...     |

**Link manufacturer first, retailer second.** Manufacturer pages document specs and survive product churn. Retailer links document price and stock, and rot faster.

**Do not lead with star ratings.** They are gameable and drift toward the median. If a rating surfaces, translate it into a specific failure mode from a low-star review.

**Do not treat Reddit consensus as the decision.** Reddit is a shortlist. Name the specific comment or thread if it changes the call.

**The same workflow applies to general procurement.** A t-shirt or a boot gets the same treatment as an M6 bolt: model or SKU, manufacturer over reseller, the two or three specs that matter (material, cut, weight, warranty), a comparison table, a one-line take, and a rejected list.

**Do not write install steps, build sequences, or test plans.** If the work reveals a plan or a design spec is needed, say so and stop. The human initiates the next phase.

**Hold the working phase open until the human closes it.** Narrowing signals ("I like X", "not Y") are not decisions. Category-agreement is not PN-agreement; PN-agreement is not quantity-agreement. Never write "locked in", "confirmed", or "final take" without an explicit close from the human. Reserve "Take" as a heading for the closing writeup; during exploration, phrase recommendations as recommendations.

**A clarifying question is exploration, not readiness to buy.** When the human asks how a product works, answer the question. Do not follow the answer with a fresh take, comparison table, or destination-choice menu.

**Procurement mode and spec mode share a session.** Procurement is the right mode when the session moves from product categories and rough cost estimates to named products, part numbers, and comparison tables. If requirements are underspecified at the start, brief spec-style category definition is appropriate before field mapping begins — no need to invoke /spec separately. If a design decision surfaces mid-session that needs spec treatment, handle it inline and return to procurement mode when resolved. The session stays open across both modes.

---

## Closing the session

The human closes the working phase, not the AI. Wait for an explicit close signal ("let's write this up", "put it in the spec", "order it", "I'm done") before offering the destination-choice menu. Do not assume a file.

Present the ask as three concrete options plus a decline:

- **Domain reference** - roll the settled decision into `<domain>_ref.md`.
- **Live project** - keep it active in `live_projects/<project>.md`.
- **Scratch** - drop it in `scratch.md` for later triage.
- **No file** - keep the conclusion in the conversation only.

Along with the destination question, name what will be written:

- The take (one line).
- The comparison table.
- Key notes and rejected paths.
- Any insight flagged as durable ("worth revisiting in 6+ months").

If the human declines a file, do not push. The conversation is the record.
If the human names a file that does not exist, confirm the path before creating it.

### Handoff signals

If the decision opens design work, note lightly that a `/spec` session is a natural next step. If it opens an install sequence, note lightly that a plan is next. Do not force the handoff - the human decides where the work goes next.

---

## Out of scope

Material handling, bill-of-materials tracking, and inventory systems are not built and are not part of this skill. If the session drifts toward stocking logic, stock categories, or inventory design, name it and stop.

---

## When this skill is no longer active

Exit procurement mode when the human explicitly signals a shift:

- "Let's plan the install."
- "Let's move to spec."
- "I'm done."

Until then, stay in procurement mode.
