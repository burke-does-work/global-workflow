---
name: cad
description: >
  Conventions and workflow for creating parametric CAD models with build123d
  (plywood panels, t-slot aluminum extrusion, and custom flat aluminum parts).
  Use this skill whenever the user is creating, editing, reviewing, or
  troubleshooting a build123d or CadQuery model, working on shop furniture,
  fixtures, enclosures, frames, or a truck shell, or working on any .py file
  that produces 3D geometry, a STEP file, or an STL — even if the user does not
  say "build123d" by name. The user is new to CAD and reads the code to
  understand the design, so dimension transparency is the top priority.
---

# build123d CAD modeling

The user is new to CAD and relies on reading the Python script to understand
and trust the design. Dimension transparency is the central goal. Every model
must read like a spec sheet from top to bottom.

## Dimension transparency — the core rule

No bare arithmetic inside geometry calls. Every number the geometry references
must be a named variable defined above it. If a value is calculated, the
calculation lives in a labeled block, never inline in a `Box()`, `Pos()`,
`extrude()`, etc.

Every model file follows this structure:

```python
# ============================================================
# BASE PARAMETERS  (raw inputs — change these)
# ============================================================
frame_width     = 600   # mm, outer width of the frame
slot_width      = 20    # mm, t-slot profile size
panel_thickness = 18    # mm, plywood
panel_clearance = 2     # mm, gap each side between panel and slot wall

# ============================================================
# DERIVED DIMENSIONS  (computed from base parameters)
# ============================================================
panel_width = frame_width - 2 * slot_width - 2 * panel_clearance  # mm

# ============================================================
# GEOMETRY  (named variables only — no inline arithmetic)
# ============================================================
with BuildPart() as panel:
    Box(panel_width, panel_depth, panel_thickness)

# ============================================================
# EXPORT
# ============================================================
export_step(panel.part, "model.step")
export_stl(panel.part,  "model.stl")

# ============================================================
# DIMENSION SUMMARY
# ============================================================
print("---- DIMENSIONS (mm) ----")
print(f"panel : {panel_width} x {panel_depth} x {panel_thickness}")
```

Rules:
- Geometry code references named variables only, never computes.
- Every variable gets a unit comment (mm) and a short plain-English note; for
  derived values, briefly say how it's derived.
- No magic numbers anywhere in geometry. If a literal must appear, name it.
- Print a labeled dimension summary at the end so the user can read real numbers
  without opening the viewer.
- Name a value when it: appears more than once, encodes a real design decision
  (clearance, wall thickness, hole spacing), or isn't self-evident. Don't name
  every trivial constant — that gets noisy.

## Workflow for every model

1. Confirm the user's t-slot extrusion brand/size if not already known (e.g.
   80/20 1010, Misumi HFS5-2020, OpenBuilds V-slot 2020). Ask once; reuse.
2. Write the model following the structure above.
3. Add `show()` calls for the OCP CAD viewer.
4. Use the build123d MCP server to render the result, measure the bounding box,
   and check key clearances. Verify geometry — don't generate blind.
5. Report bounding-box dimensions and key clearances to the user in plain text.

## Environment setup (only if not already in place)

Check PyPI and GitHub for current versions before installing — don't guess.

- Use a dedicated Python virtual environment.
- The build123d MCP server (pzfreo/build123d-mcp) has historically pinned
  Python 3.12 due to dependency wheels. Verify before choosing the interpreter.
- Install: `build123d`, `bd_warehouse` (parametric extrusion and fasteners),
  `ocp_vscode` (viewer — also runs standalone via `python -m ocp_vscode`), and
  `build123d-mcp` for MCP render/measure. Tell the user what versions you
  installed.

## Materials

- **Plywood**: 18 mm default unless specified. Always a base parameter.
- **T-slot extrusion**: use bd_warehouse profiles when available (modeled on
  OpenBuilds system). If the user's brand isn't covered, generate a
  dimensionally correct custom profile and say so — never silently substitute.
- **Custom flat aluminum parts**: model as flat stock with named hole-pattern
  parameters (diameter, edge offset, spacing) since these are the parts the
  user drills.

## Communication

Briefly explain each major step. Flag anything uncertain (version pins, profile
availability, clearance assumptions) rather than guessing. State assumptions
inline so the user can see and override them.
