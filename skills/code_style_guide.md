---
name: code_style_guide
description: Project style conventions and code standards. Load when writing or reviewing code.
---

<!-- Skill file for Claude Code. Listed in AGENTS.md but not auto-loaded — Claude reads this file when a task involves writing or reviewing code. -->

Custom addendums always take precedence over base style guides.

## General guidance

Code should be easy to read and manually review.

### Code writing

- Write beginner/intermediate level Python and SQL code
- Avoid clever abstractions or complex one-liners
- Prefer explicit over implicit code
- Avoid large `lambda` functions. Prefer long-form function definition. But use in pandas to facilitate method chaining

### Project structure

- Functions in other scripts are imported as modules or as individual functions from a module, not via packaged libraries.

### Comments in scripts

- Comment to explain intent
- Script should be well-commented but not verbose

## Python

**Base**: PEP 8
**Formatter**: Black (line length 88)

### Docstrings (Python)

- Use triple double quotes """ and always write docstrings in multi-line form. The closing """ must be on its own line.
- Required parts, in order:
  - One-line summary (first line; sentence; ends with a period)
  - Blank line (between summary and next section)
  - Returns
- Optional:
  - longer description following one-line summary, as required

Template:

```Python
def func(...) -> ...:
    """Summary.

    Optional description.

    Returns:
        What the function returns.
    """
```

### Error Handling

- Fail early and loudly. Raise on invalid inputs, schema, config.
- Use clear messages but succinct messages.
- Prefer exceptions. Do not use values for errors.
- Check data and parameters when they enter the system.
- Do not add error handling everywhere. After validation, keep core logic simple and avoid repetitive defensive checks.

### Pandas

The intent is to prefer patterns that make it obvious what data you're changing, minimize ambiguity around views vs. copies, and reduce the chance of hard-to-debug behavior. This guidance prioritizes clarity and correctness over micro-optimizations, and should be consistent with the general guidance in this doc.

- Selecting and updating existing data:
  - Prefer `.loc[...]` when selecting and updating rows/columns.
  - Avoid chained selections like `df[mask]["col"] = ...` (can trigger `SettingWithCopyWarning` and may not update what you expect).

- Avoid `inplace=True` by default:
  - Prefer `df = df.method(...)` or `df["col"] = df["col"].method(...)` over `inplace=True` variants.
  - Rationale: `inplace=True` is harder to read and edit.

- Adding / transforming columns (derived data):
  - Prefer **method chaining** for multi-step transformations (pipelines). Makes the sequence of operations explicit and reduce intermediate mutations.
    - Prefer `.assign(...)` for adding/overwriting derived columns inside chains.
    - Prefer `.pipe(...)` to break a long chain into named, testable steps.
    - Split chains based on code logic and for `return` statements.
  - `df["new"] = ...` is **allowed but secondary** for small changes.
- Prefer vectorized operations (`fillna`, `where`, arithmetic on Series, `astype`, etc.) over `apply(...)` for simple elementwise logic.

- Combining DataFrames:
  - Prefer `pd.concat(...)` for stacking/concatenation. Do not use `DataFrame.append` (deprecated).
  - Prefer `merge` when matching data from another table by keys.

- Copying:
  - Avoid `.copy()` by default, except where to explicitly establish a copied DataFrame.
  - Do not use `.copy()` as a quick fix to silence `SettingWithCopyWarning`.
  - Write the code to avoid chained assignment (typically with `.loc` or `.assign`).

- Iteration:
  - Avoid `iterrows()` (and cell-by-cell loops) for data changes.
  - Prefer `.loc`, `.assign`, vectorized operations, `merge`/`join`, and `map` (for lookup dict/Series) for updates/fills.
  - If the data is small and a loop is clearest, a loop is OK—just don't update the DataFrame one cell at a time inside the loop.

- Readability vs. vectorization:
  - Prefer vectorized solutions when they remain readable.
  - If a vectorized solution is significantly harder to read/maintain, choose the clearer approach.
  
- Naming in chained transforms:
  - In `.assign(..., lambda ...)`, use `d` for DataFrame and keep it consistent within the chain.
    - Example: `.assign(x=lambda d: ..., y=lambda d: ...)`

### Custom addendum

- Use `# ===...===` section banners to separate major sections (e.g., constants, queries, main)
- For sub-sections, use `# --- foo ---`

- Prefer f-strings for string formatting

## SQL

**Base**: [dbt Labs style guide](https://docs.getdbt.com/best-practices/how-we-style/2-how-we-style-our-sql)

### Custom addendum

- 4-space indent (dbt Labs default easy to miss)
- Grouped column sets (e.g., HE1–HE24) may appear on a single line as an exception to one-column-per-line
