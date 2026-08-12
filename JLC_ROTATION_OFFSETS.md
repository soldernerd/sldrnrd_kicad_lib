# JLCPCB Rotation Offsets

## Purpose

JLCPCB's SMT assembly service maintains its own internal "default orientation"
record for every part in its catalog, keyed by LCSC part number. When this
internal orientation disagrees with the actual pin-1 orientation used by a
footprint in this library, JLCPCB's assembly-preview renders the part rotated
incorrectly relative to how it is (correctly) placed in KiCad.

This is a property of the **part** (as identified by its LCSC number / MPN),
not of any particular board design. This file documents known corrections so
that future CPL/position-file exports — for any project that reuses these
parts — can pre-apply the correction instead of rediscovering it after
placing an order and checking the assembly preview.

These corrections are applied only when generating the CPL (Component
Placement List) file submitted to JLCPCB. They do not affect the KiCad
schematic, footprint, or PCB layout, which remain correct as-is.

## Rotation sign convention

KiCad/pcbnew rotation angles are **positive = counter-clockwise (CCW)** when
viewed from the top/front of the board.

The **"Correction Needed"** column below is the offset to **add (mod 360)**
to the KiCad-derived CPL rotation value for that part, in order to get the
rotation angle JLCPCB expects. For example, if KiCad's derived CPL rotation
for a given placement is `45` degrees and the correction needed is `-90`,
submit `-45` (i.e. `315`) degrees to JLCPCB for that part.

Because "clockwise" and "counter-clockwise" are easy to flip by mistake when
translating between conventions, each row also records the plain-English
direction as it was originally observed/reported, so the numeric sign can
always be cross-checked against it.

## Known corrections

| LCSC Part # | MPN | Manufacturer | Footprint | Correction Needed | Notes |
|---|---|---|---|---|---|
| C2829307 | STM32G0B1RET6 | STMicroelectronics | LQFP-64 | -90 (equiv. +270) | Observed: JLCPCB assembly preview needed the part rotated 90 degrees CW relative to the KiCad-derived CPL rotation for correct physical orientation. CW = negative in the CCW-positive convention used here. |
| C172354 | HX4002-MFC | HEXIN Semiconductor | SOT-23-6 | 180 | Observed: JLCPCB assembly preview needed the part rotated 180 degrees relative to the KiCad-derived CPL rotation. Direction (CW/CCW) is irrelevant at 180 degrees. |
| C2830320 | FS8205A | TECH PUBLIC | SOT-23-6 | 180 | Observed: JLCPCB assembly preview needed the part rotated 180 degrees relative to the KiCad-derived CPL rotation. Direction (CW/CCW) is irrelevant at 180 degrees. |
| C351410 | DW01A | PUOLOP | SOT-23-6 | -90 (equiv. +270) | Observed: JLCPCB assembly preview needed the part rotated 90 degrees CW relative to the KiCad-derived CPL rotation for correct physical orientation. CW = negative in the CCW-positive convention used here. |
| C2286 | KT-0603R | KENTO | LED_0603 | 180 | Observed: JLCPCB assembly preview needed the part rotated 180 degrees relative to the KiCad-derived CPL rotation. Direction (CW/CCW) is irrelevant at 180 degrees. |
| C5563894 | FTSH-107-01-L-DV-K-A | Samtec | Header_50mil_2x07 | 180 | Observed: JLCPCB assembly preview showed the single-long-wall/pin-1 side on the opposite end from the correct KiCad placement. Direction (CW/CCW) is irrelevant at 180 degrees. |

## Important caveat

These are **empirically observed** corrections, first discovered on the
Levelmeter board project, derived by comparing JLCPCB's assembly-preview
render against the known-correct KiCad placement for each part. They are
recorded here per LCSC part number because they are believed to be a
property of JLCPCB's internal orientation database for that specific part,
not of any specific board or footprint instance.

However, JLCPCB's internal orientation database is not under our control and
could change over time, or a given LCSC part number could be re-mapped to a
different physical part revision. If a future board places any of these same
parts and JLCPCB's assembly preview still looks wrong (or looks wrong in a
different way than described above), **re-verify against the preview rather
than assuming these offsets are universally exact.** Update this table if a
correction is found to have changed.
