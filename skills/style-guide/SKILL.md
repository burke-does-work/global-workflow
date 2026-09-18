---
name: style-guide
description: Global style guide - writing conventions, formatting, units of measurement, and document structure.
---

## Tooling

Prettier formats and markdownlint lints. Where a repo runs them, they define every convention they cover, and those rules are not restated here. Follow them when writing rather than relying on a format pass to clean up afterwards.

Everything below is applied by hand. No tool checks it.

---

## Headings

- Use heading tags for headings only.
- The H1 matches the title in the YAML front matter.

### Capitalization

- H1: Title Case.
  Capitalize each word except articles, conjunctions, and prepositions, unless the word starts the heading or follows a dash or colon.
  - Examples: "The Quick Brown Fox Jumps Over the Lazy Dog"; "Publishing Workflow: A Guide for Writers".
- H2-H5: Sentence case.
  Capitalize only the first word, or a word after a dash or colon.
  - Examples: "Publishing workflow"; "Image handling and path management".

## Wrapping

Do not hard-wrap prose in Markdown: keep each paragraph and list item on one source line, and let the editor or renderer wrap it visually.

---

## Numbering and lists

The purpose is to enable easy refactoring of lists and steps, consistency, and human edits.

### Lists

- Prefer unordered lists to ordered lists.
- No blank line between a parent list item and its nested sub-items.
  A blank line makes the list "loose" and wraps items in `<p>` tags when rendered to HTML.

### Numbers

- In addition to list preferences, do not use numbering for steps or stages, including in headings.
- For example, use "Stage: Setup style guide", not "Stage 1: Setup style guide".

### Task lists

- Avoid task lists.
- Where status should be indicated, use a word such as "done", "open", or "deferred".

---

## Sections

- Use a horizontal rule (`---`) between sections introduced by a level-two heading (`##`).

---

## Punctuation and symbols

Use ASCII. Unicode typographic characters are awkward to reach on a keyboard and mostly arrive uninvited from editor autocorrect, so turn smart substitution off rather than cleaning up after it.

Watch for the non-breaking space (U+00A0). It is the one violation you cannot see - invisible in a diff, and it breaks search. macOS inserts it on option+space.

### Dashes

- Joined words and spans: a hyphen, no spaces either side - `well-known`, `10-20`, `Mon-Fri`.
- A break in a sentence: a hyphen with a space either side. It offsets an aside or stands in for a semicolon.
- `---` is structural only - the break between sections, and front matter fences. Never punctuation.

### Exceptions

Use the real character in two cases.

Where no ASCII form exists:

- Non-Latin scripts.
- Currency symbols - `£`, `€`, `¥`.
- The micro sign in unit symbols - `µm`, `µF`.

Where an ASCII form exists but is wrong:

- Diacritics in words from languages that use them, common nouns and names alike - `Gemütlichkeit`, `piñata`, `Gödel`. Stripping one misspells the word.

Where English has naturalized a spelling without the diacritic - `cafe`, `naive`, `uber` - that is an English word rather than a substitution, and the style hierarchy decides it.

This does not reopen cases settled elsewhere: the units table takes `deg C` over the degree sign, and ASCII `'` and `"` over the prime marks.

---

## Tables

Tables suit items compared on shared attributes, where the eye scans down a column.

- Cells hold fragments, not sentences.
- Past six columns, split the table or switch to a list.

Detail that outgrows a cell goes in prose above or below the table. Where the content is long throughout, prefer a list.

---

## Units of measurement

SI units follow NIST SP 811: one space between numeral and unit symbol, no trailing period, no plural suffix.

SAE units (lb, ft, in, mph) follow the same no-punctuation approach for consistency with SI.

Percentage follows Chicago Manual of Style: attached to the numeral with no space (`50%`).

| Unit            | Symbol | Example                                            |
| --------------- | ------ | -------------------------------------------------- |
| Millimeters     | mm     | `1 mm`                                             |
| Meters          | m      | `1 m`                                              |
| Kilograms       | kg     | `1 kg`                                             |
| Pounds          | lb     | `1 lb` (not `lbs`, not `lb.`)                      |
| Feet (symbol)   | '      | `1'` (ASCII U+0027, not prime mark)                |
| Inches (symbol) | "      | `1"` (ASCII U+0022, not prime mark)                |
| Feet (prose)    | ft     | `1 ft`                                             |
| Volts           | V      | `1 V`                                              |
| Watts           | W      | `1 W`                                              |
| Amperes         | A      | `1 A`                                              |
| PSI             | psi    | `30 psi` (lowercase)                               |
| Celsius         | deg C  | `10 deg C` (informal; `°` is a nuisance character) |
| Percentage      | %      | `50%`                                              |
| Newton-meters   | Nm     | `44 Nm`                                            |

Compound units use a solidus with no surrounding spaces; the space goes before the whole compound: `14 W/m`.

Dual-unit specs: space before each symbol: `44 lb/20 kg`.

Prose: `1 ft`, `1 inch`, `6 foot leash`, `degrees Celsius` are all acceptable in running text.

---

## Keybindings

- Modifier names: all lowercase - `ctrl`, `alt`, `shift`, `super`, `hyper`.
- Key names: all lowercase - `ctrl+c`, `ctrl+shift+t`, `alt+space`.
- Separator: `+` with no surrounding spaces.
- Multi-key chords: space between them - `ctrl+k ctrl+p`.
- Multiple shortcuts on one line: separate with a slash surrounded by single spaces - `` `ctrl+u` / `ctrl+d` ``.
- Always wrap keybindings in backticks.

---

## Style hierarchy

For anything this guide does not explicitly cover, apply these in order:

- [BuzzFeed Style Guide](https://www.buzzfeed.com/buzzfeednews/buzzfeed-style-guide)
- [Associated Press Stylebook](https://www.apstylebook.com/)
- [The Chicago Manual of Style](https://www.chicagomanualofstyle.org/home.html)

Use American English. Vocabulary is flexible: Australian-isms are not flagged in a style check.

For Chinese-language writing, use [The China Story Style Guide](https://www.thechinastory.org/submission-guide/style-guide/).

For Markdown, default to [CommonMark](https://spec.commonmark.org/0.31.2/) with [GitHub Flavored Markdown](https://github.github.com/gfm/) extensions.

---

## Related

- Prose voice and language conventions: `/voice`
- Code and docstring conventions: `/code-guide`
