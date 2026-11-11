# Amy Helix Spire — FreeCAD model

This repository contains a reproducible, dimensioned FreeCAD model of a 150 mm
resin obelisk. It creates four named solid components: `DecorativeBase`,
`ResinBody`, `HelixRibbon`, and `CrystalCore`.

The ribbon is a solid loft through oriented rectangular sections. Its centreline
does not use a fixed cylindrical radius: at every sample it intersects a rotated
ray with the current square-taper half-width, then adds 2.2 mm clearance and half
the 2.4 mm ribbon thickness.

## Files

- `cad/Amy_Helix_Spire.FCMacro` — authoritative generator and validation checks
- `cad/Amy_Helix_Spire_parameters.json` — machine-readable parameter mirror
- `cad/Amy_Helix_Spire.FCStd` — generated only when every macro check passes
- `validation/geometry_checks.md` — recorded execution evidence and limitations

## Open and inspect

Open `cad/Amy_Helix_Spire.FCStd` in FreeCAD 1.1.4. Select components in the tree
to inspect their Data properties; use View → Panels → Property view if needed.
The resin body encloses the offset six-sided crystal, while the separate ribbon
follows the exterior taper.

## Regenerate in the FreeCAD GUI

1. Do not close any existing document or FreeCAD session.
2. Choose **Macro → Macros… → Create**, use any temporary macro name, and paste
   the contents of `cad/Amy_Helix_Spire.FCMacro` into the editor; alternatively,
   add the repository's `cad` directory under **Edit → Preferences → Python →
   Macro → Macro path**.
3. Run `Amy_Helix_Spire.FCMacro` from **Macro → Macros…**.
4. Read the Report view for `CHECK ...=PASS` lines. The macro saves the FCStd
   only if all mandatory checks pass.

The `Parameters (rerun macro to regenerate)` object exposes editable properties
for convenient inspection. They are **not live-linked** to the solids. The macro
constants at its top remain authoritative; property edits require updating those
constants and rerunning the macro.

No STEP export, rendering, fabrication work, material testing, or changes to
external FreeCAD/NZGPTS files are part of this task.

