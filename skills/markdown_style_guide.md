---
name: markdown_style_guide
description: Global Markdown conventions. Load when writing or reviewing Markdown.
---

Custom addendums always take precedence over the base style guides.

## Base

Default to [CommonMark](https://spec.commonmark.org/0.31.2/).
Where CommonMark is silent, defer to the [markdownlint rules](https://github.com/DavidAnson/markdownlint/blob/main/doc/Rules.md).

---

## Headings

- Use heading tags for headings only.
  Do not use emphasis (bold) as a heading.
  Where a formatted heading is not wanted, use plain text with a colon.
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

## Sentences

- One sentence per line, unless that reduces clarity.
  Keeps diffs clean.

---

## Lists

- Prefer unordered lists to ordered lists.
- Use "-" for unordered list markers.
  Do not mix in "*".
- Single blank line between a list block and the text above and below it.
- No blank line between a parent list item and its nested sub-items.
  A blank line makes the list "loose" and wraps items in `<p>` tags when rendered to HTML.

---

## Sections

- Use a horizontal rule (`---`) between sections introduced by a level-two heading (`##`).

---

## Dashes

- Use "-", not "—".

---

## File ending

- End every document with a newline.
