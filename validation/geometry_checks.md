# Geometry validation

Execution date: 2026-11-10 (Pacific/Auckland). The macro was run with the
supplied `FreeCAD_1.1.4-Linux-x86_64-py311.AppImage`. Because FUSE is unavailable
in the execution environment, the AppImage was started with
`APPIMAGE_EXTRACT_AND_RUN=1` and `QT_QPA_PLATFORM=offscreen`; this uses the
supplied FreeCAD binaries without installing or reconfiguring the system.

## Required checks

- Shapes: non-null, valid, closed, exactly one solid, positive volume
- Assembly envelope: 150 mm height, 50 × 50 mm footprint, z minimum 0 mm
- Ribbon: sampled centreline agrees with the rotated-ray/square-taper equation,
  has no boolean overlap with the body, and has positive boundary clearance
- Crystal: its boolean intersection with the body equals its full volume and its
  boundary has positive clearance from the body boundary
- Saved model: reopen in FreeCAD 1.1.4 and repeat shape/envelope checks

## Final generator result

All mandatory macro checks passed and the macro saved
`cad/Amy_Helix_Spire.FCStd`.

| Check | Actual result |
|---|---:|
| FreeCAD version on independent reopen | 1.1.4 |
| Assembly bounding box | 50.000000 × 50.000000 × 150.000000 mm |
| Assembly minima | x = -25.000000, y = -25.000000, z = 0.000000 mm |
| `ResinBody` | valid, closed, 1 solid, 140450.000000 mm³ |
| `HelixRibbon` | valid, closed, 1 solid, 4277.832060 mm³ |
| `CrystalCore` | valid, closed, 1 solid, 9166.012874 mm³ |
| `DecorativeBase` | valid, closed, 1 solid, 17210.000000 mm³ |
| Ribbon/body minimum boundary distance | 0.282769 mm |
| Ribbon/body boolean common volume | 0.000000000 mm³ |
| Ribbon x range | -23.136213 to 24.872033 mm |
| Ribbon y range | -21.971774 to 24.304332 mm |
| Crystal/body minimum boundary distance | 6.775937 mm |
| Crystal/body common-volume ratio | 1.000000000 |

The saved document independently reopened as `Amy_Helix_Spire` and contained
the named objects `ResinBody`, `HelixRibbon`, `CrystalCore`, `DecorativeBase`,
and `Parameters`. Every component repeated the valid/closed/single-solid and
positive-volume checks after reopening.

The centreline runs from z = 18 mm to z = 132 mm for 1.5 turns. At each sample,
its radius is `square_half_width / max(abs(cos(theta)), abs(sin(theta)))`, plus
2.2 mm clearance and half the 2.4 mm thickness. The 145-section loft is a valid
solid; FreeCAD reported no self-intersection through `Shape.isValid()`.

## Attempts that did not pass

- The first containment check incorrectly expected zero boundary distance for a
  fully enclosed crystal. It was replaced with full boolean-volume containment
  plus positive boundary clearance.
- Early 73-section/0.8 mm and 145-section/1.8 mm ribbon trials clipped the body.
  A 2.0 mm trial exceeded the 50 mm footprint by 0.132728 mm. These files were
  not retained as the final model.
- The accepted geometry uses a 41 mm lower shaft seated on the plinth, 145
  ribbon sections, and 2.2 mm nominal centreline clearance. It passes both zero
  overlap and the 50 mm envelope.

## Limitations and failures

- No STEP file was generated.
- The document was generated and reopened using FreeCAD 1.1.4 in offscreen
  console mode. Geometry and document loading are proven; interactive GUI
  display was not exercised because this environment has no display server.
- Document parameter properties are editable metadata, not live constraints.
  Geometry changes require editing the macro's top-level constants and rerunning
  it, as also stated on the `Parameters` object.

To view the model, launch the supplied FreeCAD normally and choose **File →
Open**, then select `cad/Amy_Helix_Spire.FCStd`. This does not require rerunning
the macro.

