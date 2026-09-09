---
name: style-guide
description: Global style guide — writing conventions, formatting, units of measurement, and document structure.
---

Custom addendums always take precedence over the base style guides.

## Base

Default to [CommonMark](https://spec.commonmark.org/0.31.2/).
Where CommonMark is silent, defer to the [markdownlint rules](https://github.com/DavidAnson/markdownlint/blob/main/doc/Rules.md).

---

## Headings

- Use heading tags for headings only.
  Do not use emphasis (bold) as a heading -- a standalone bolded line, on its own with no other text, that stands in for a heading tag to introduce what follows.
  Where a formatted heading is not wanted, use plain text with a colon instead of a bolded line.
  Bold is otherwise unrestricted: an inline lead-in label sharing a line with its content (e.g., "**Note:** ...", "**Purpose**: ...") is not a heading and does not fall under this rule.
- Increment heading levels by one at a time.
- The H1 matches the title in the YAML front matter.

### Capitalization

- H1: Title Case.
  Capitalize each word except articles, conjunctions, and prepositions, unless the word starts the heading or follows a dash or colon.
  - Examples: "The Quick Brown Fox Jumps Over the Lazy Dog"; "Publishing Workflow: A Guide for Writers".
- H2-H5: Sentence case.
  Capitalize only the first word, or a word after a dash or colon.
  - Examples: "Publishing workflow"; "Image handling and path management".

---

## Numbering and lists

The purpose is to enable easy refactoring of lists and steps and consistency.

### Lists

- Prefer unordered lists to ordered lists.
- Use "-" for unordered list markers.
  Do not mix in "*".
- Single blank line between a list block and the text above and below it.
- No blank line between a parent list item and its nested sub-items.
  A blank line makes the list "loose" and wraps items in `<p>` tags when rendered to HTML.

### Numbers

- In addition to list preferences, do not use numbering for steps or stages, including in headings.
- For example, use "Stage: Setup style guide", not "Stage 1: Setup style guide".

---

## Sections

- Use a horizontal rule (`---`) between sections introduced by a level-two heading (`##`).

---

## Typographic substitutions

Prefer ASCII sequences over Unicode typographic equivalents. Do not rely on automatic substitution. Common cases:

- Dashes: "--" or "---", not "—".
- Arrows: "->", not "→".

---

## File ending

- End every document with a newline.

---

## Units of measurement

SI units follow NIST SP 811: one space between numeral and unit symbol, no trailing period, no plural suffix.

SAE units (lb, ft, in, mph) follow the same no-punctuation approach for consistency with SI.

Percentage follows Chicago Manual of Style: attached to the numeral with no space (`50%`).

| Unit | Symbol | Example |
|---|---|---|
| Millimeters | mm | `1 mm` |
| Meters | m | `1 m` |
| Kilograms | kg | `1 kg` |
| Pounds | lb | `1 lb` (not `lbs`, not `lb.`) |
| Feet (symbol) | ' | `1'` (ASCII U+0027, not prime mark) |
| Inches (symbol) | " | `1"` (ASCII U+0022, not prime mark) |
| Feet (prose) | ft | `1 ft` |
| Volts | V | `1 V` |
| Watts | W | `1 W` |
| Amperes | A | `1 A` |
| PSI | psi | `30 psi` (lowercase) |
| Celsius | deg C | `10 deg C` (informal; `°` is a nuisance character) |
| Percentage | % | `50%` |
| Newton-meters | Nm | `44 Nm` |

Compound units use a solidus with no surrounding spaces; the space goes before the whole compound: `14 W/m`.

Dual-unit specs: space before each symbol: `44 lb/20 kg`.

Prose: `1 ft`, `1 inch`, `6 foot leash`, `degrees Celsius` are all acceptable in running text.

---

## Keybindings

- Modifier names: all lowercase - `ctrl`, `alt`, `shift`, `super`, `hyper`.
- Key names: all lowercase - `ctrl+c`, `ctrl+shift+t`, `alt+space`.
- Separator: `+` with no surrounding spaces.
- Multi-key chords: space between them - `ctrl+k ctrl+p`.
- Multiple shortcuts on one line: ` / ` between them - `` `ctrl+u` / `ctrl+d` ``.
- Always wrap keybindings in backticks.

---

## Style hierarchy

For anything this guide does not explicitly cover, apply these in order:

- [BuzzFeed Style Guide](https://www.buzzfeed.com/buzzfeednews/buzzfeed-style-guide)
- [Associated Press Stylebook](https://www.apstylebook.com/)
- [The Chicago Manual of Style](https://www.chicagomanualofstyle.org/home.html)

Use American English. Vocabulary is flexible: Australian-isms are not flagged in a style check.

For Chinese-language writing, use [The China Story Style Guide](https://www.thechinastory.org/submission-guide/style-guide/).

---

## Related

- Prose voice and language conventions: `/voice`
- Code and docstring conventions: `/code-guide`
