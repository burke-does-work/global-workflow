---
name: dashu-me
description: "Dashu's personal visual taste filter for frontend/UI/product design. Load when designing interfaces, reviewing frontend work, or when the user wants a more uniquely Dashu aesthetic."
---

# dashu-me

Use this skill to make visual design feel more like Dashu:

- Independent field-builder energy
- utilitarian
- frank
- personal
- no bullshit
- inter-disciplinary

This is a taste filter that should shape the work before generic frontend polish is applied.

## North Star

Make the interface feel like it was built by a thoughtful lone wolf who understands people across functions and interests. Simple enough to trust, distinct enough to remember, and alive enough to feel conversational.

The default goal is not "beautiful." The goal is: **the intelligent layperson understands it quickly and want to engage**

## Core Taste DNA

- **Independent field-builder energy over corporate polish** - favor intellectual curiosity with long term goals. Specificity and and crafted details, with a sense that a real builder made this.
- **Builder authentic over tech theater** - visual languages that respects the intelligence of the audience: structure, clarity, honest, details that reward close inspection. 
- **Simple first, memorable second** - Reduce taxonomy, copy, and layout complexity before adding limited flair. If the core idea is muddy, no amount of flash fixes it.
- **Restrained, crisp motion** - Add more animation than a sterile enterprise UI would, but keep it fast, execution-driven, and responsive.
- **Selective whimsy** - Include small moments of personality, not constant decoration.
- **Premium utilitarian materials** - Borrow from well-engineered and high-quality workshop tools: comfortable, precise, durable, compact, matte, tactile, quietly capable.

## Strong Enemy

Fight generic AI/SaaS defaults aggressively:

- Purple-blue gradients as the main idea
- Glowing orbs, glassmorphism, neon halos, and vague "AI magic" effects
- Centered hero + dark mesh background + three equal feature cards
- Decorative sparkles with no product meaning
- Over-polished, corporate, safe blandness
- Default Inter/Roboto/system-font pages that feel like a template
- Copy that says "revolutionize," "unlock," "seamless," or "supercharge" without product evidence
- Motion that loops forever just to seem advanced

When you see these patterns, say so directly and replace them with a more specific product idea.

## Default Design Dials

Use these defaults unless the brief clearly requires otherwise:

| Dial                 | Default | Meaning                                                          |
| -------------------- | ------: | ---------------------------------------------------------------- |
| Simplicity           |    8/10 | Cut hierarchy, labels, and visual noise                          |
| Independence         |    8/10 | Should feel unique, untethered by norms                          |
| Builder authenticity |    9/10 | Respect technical users; no fake AI mysticism                    |
| Motion intensity     |    3/10 | More animated than enterprise SaaS, less                         |
| Signature detail     |    3/10 | A few, small memorable moments, never everywhere                 |
| Visual density       |    4/10 | Enough structure to feel useful, enough air to feel approachable |
| Corporate polish     |    2/10 | Avoid boardroom-safe blandness unless explicitly required        |

## Required Design Read

Before making or revising UI, state one concise design read:

> Reading this as: `<surface>` for `<audience>`, leaning into `<independent field-builder direction>`, with `<simple motion/whimsy plan>`.

If the brief is ambiguous, ask one focused question. Otherwise, proceed and make an opinionated call.

## Visual Direction

Prefer:

- Clear product surfaces over abstract marketing art
- Slightly asymmetric layouts that still scan quickly
- One strong visual metaphor tied to the product, not a generic theme
- Honest UI artifacts: command bars, cards, traces, diffs, logs, receipts, timelines, maps, workbenches, notebooks, tiny diagrams
- Warm neutral foundations with one accent
- Matte, tactile, paper/tool/material cues instead of glossy glass
- Characterful typography for headings paired with highly readable body text
-Visual cues: stamps, labels, annotations, handoff notes, status chips, small diagrams, material texture, and useful details that feel like they came from the field, the shop, or the margin of a working notebook. Small maker marks: stamps, labels, annotations, handoff notes, status chips, subtle texture

Avoid:

- Feature grids that could belong to any startup
- Decorative dashboards with fake charts
- Empty visual metaphors like "constellation of intelligence"
- Pure minimalism that becomes sterile
- Maximalism that hides the product idea

## Motion Rules

Animation should make the app more interesting while staying simple.

### Default motion personality

- **Intelligent but crisp**: quick, responsive, and easy to interrupt.
- **Signature detail in selected moments**: a delightful signature detail when something completes, appears for the first time, or teaches the user what changed.
- **Never sluggish**: no slow cinematic transitions for repeated actions.

### Timing defaults

| Motion type                   |   Default |
| ----------------------------- | --------: |
| Button/tap feedback           | 100-160ms |
| Hover/focus feedback          | 120-180ms |
| Small popovers/tooltips       | 140-220ms |
| Cards/toasts/dialog entrances | 180-280ms |
| Rare whimsical moments        | 300-600ms |

### Preferred techniques

- Animate `transform`, `opacity`, and occasionally `filter` for small local elements.
- Use short staggered reveals for grouped content.
- Use tactile press states: subtle scale, shift, or snap.
- Use origin-aware motion: popovers should emerge from the trigger; panels should move from their spatial source.
- Add small completion moments: checkmark draw, card settle, label stamp, tiny object motion, or a measured copy change.
- Respect `prefers-reduced-motion`; keep whimsy optional or simplified.

### Where to add whimsy

Good places:

- Empty states
- First-run/onboarding moments
- Successful task completion
- Loading states that would otherwise feel dead
- Mode switches
- Small celebratory moments after user effort
- Product-specific easter eggs

Bad places:

- Keyboard-heavy workflows used dozens of times per day
- Destructive confirmations
- Error states that need calm clarity
- Security, billing, or compliance surfaces
- Dense data workflows where animation slows comprehension

## Copy Voice

Copy should be crisp, direct, and slightly human. It should sound like a builder explaining what matters, not a brand campaign.

Prefer:

- "Show the prototype."
- "Nothing to review yet."
- "Your next move is ready."
- "This changed because..."
- "Make developers trust us again."

Avoid:

- "Unlock seamless productivity."
- "Harness the power of AI."
- "Revolutionize your workflow."
- "Supercharge your development journey."

## Critique Style

Be direct and opinionated. Call out blandness.

Use this pattern:

1. **What feels generic** - Name the default or trope.
2. **Why it weakens trust or memory** - Explain the product/design cost.
3. **What to do instead** - Give a concrete replacement.

Example:

> This still feels like a generic AI SaaS landing page: dark gradient hero, centered headline, and interchangeable cards. Replace the abstract glow with a product-specific command surface, add one tactile completion animation, and make the headline say exactly what the user can do next.

## Output Format for Design Work

When applying this skill, include:

1. **Design read** - One sentence.
2. **Taste direction** - The product-led visual idea.
3. **Motion plan** - What animates, why, and where whimsy appears.
4. **Anti-generic changes** - The specific defaults being avoided.
5. **Implementation** - Make the design real in code or concrete artifact edits when asked to build.

Keep the explanation concise unless the user asks for a full rationale.

## Pairing With Other Skills

- Use with `frontend-design`
- Use with `web-artifacts-builder` when creating HTML artifacts, dashboards, visualizations, or interactive tools.

This skill should usually run before broad polish skills so the design has a personal point of view before it becomes refined.