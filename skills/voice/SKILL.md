---
name: voice
description: Global prose voice, audience standard, and language conventions. Load when writing or reviewing prose (notes, docs, emails, reports).
---

This is the source of truth for my written voice.
It applies to all prose unless a project explicitly overrides it.

## TL;DR

- Write for an intelligent layperson who has the internet and an LLM.
- Use first-person ("I") and the imperative.
- Do not use second-person ("you"); prefer "a person".
- Use "recommend" when one of several viable options is preferred.
- Do not use passive voice.
- Prefer conjunctions.
- Spell out acronyms unless they are well known.

---

## Audience and communication

Write to the intelligent layperson standard.
The reader is smart but does not already hold the specific knowledge or thought process behind the topic.

- Provide brief context when it is needed, then move on.
- Push extra detail or resources into a footnote when it adds value over what is already on the internet.
- Assume the reader has internet access and is adept with Google or an LLM for more information.

Aim for insight over briefing.
Favour a conceptual guide over step-by-step mechanics that a person can look up.

---

## Person, not "you"

I do not like being told what to do, and unsolicited advice is rarely welcome.
Avoid "you" and "you should".
"You" is also ambiguous once a note is shared, even when it was meant for me.

Use a combination of first-person ("I") and the imperative.
When neither fits, use "a person" rather than a formal "one".

Other workable substitutes for "you":

- "Anyone" - conversational, good for tutorial or blog-style writing.
- "People" - very conversational, like explaining something out loud.
- Role-based ("the user", "most devs") - when the group is specific.

### Recommend

Use "recommend" when there are multiple viable options but one is preferred.
Use it in a declarative sentence.

- Example: "I recommend using the latest version of Flask."

### "You" in technical documentation

Technical documentation commonly uses "you", and that is acceptable in that context.
For example, installation docs that say to create a project folder and a `.venv` within it.
"We recommend" is the usual form when options exist but one is best.

---

## Sentences

- Do not use passive voice.
- Prefer conjunctions, unless there is a good reason not to use one.

---

## Acronyms

Spell out the full word or phrase in parentheses, unless the acronym is well known to the general population.

- Spell out: "SDLC (Software Development Life Cycle)" or "Software Development Life Cycle (SDLC)".
- No need to spell out: "JFK" needs no expansion.

---

## Language and prose style guides

Apply these in order, the first taking precedence:

1. [BuzzFeed Style Guide](https://www.buzzfeed.com/buzzfeednews/buzzfeed-style-guide)
2. [Associated Press Stylebook](https://www.apstylebook.com/)
3. [The Chicago Manual of Style](https://www.chicagomanualofstyle.org/home.html)

Use American English.
Vocabulary is flexible: Australian-isms are not flagged in a style check.

For Chinese-language writing, use [The China Story Style Guide](https://www.thechinastory.org/submission-guide/style-guide/).

---

## Related

- Markdown conventions: `/md-guide`
- Code and docstring conventions: `/code-guide`
