# Calibration

The mat is the instrument. The A4s teach it to tell the truth.

## Why three A4s

A tag-on-mat photo cannot separate “the printer scaled the sheet to 98%” from “the mat’s inch is 25.0 mm.” So:

1. Trust the **print** (with one check).
2. Solve for the **mat**.

Three uniquely-ID’d sheets give metric islands at three regions (projector distortion is worse in the corners than the centre) and leave enough exposed grid to fit a lattice.

## CalSheet v1

See [spec/calsheet.v1.md](../spec/calsheet.v1.md). Short version:

- A4, 10 mm quiet margin, matte copy paper.
- Charuco 5×7 @ 28 mm. Sheet A IDs 0–19, B 20–39, C 40–59.
- 100.00 mm bar. 50 mm square. Credit-card ghost (85.60 × 53.98 mm).
- Huge “PRINT AT 100% / DO NOT SCALE”.

## Print-scale check (pick one)

1. User types what a ruler says the 100 mm bar measures. Best.
2. Photograph a credit card in the ghost box. Good.
3. “I printed at 100% and I trust it.” Acceptable.

That number `s` applies to all three sheets from that print batch.

## Placement

On a 24×36″ mat: A top-left, C centre, B bottom-right. Do not overlap, do not hang off the edge. Alignment to the grid is nice, not required.

On 18×24″: same idea, tucked in.

## Solve

1. Detect Charuco per sheet → three `paper-mm → camera` homographies, scaled by `s`.
2. Detect the mat lattice in the gaps (LSD / constrained Hough). Two vanishing points. Drop 45° bias lines — they fail the “two-family axis-aligned lattice” test.
3. Everything lives on one plane. Fit:
   - table origin (a chosen grid intersection)
   - axis angle
   - **pitch X and pitch Y in millimetres** ← this is “auto-adjust the board”
   - a small affine residual (cheap mats are not rectangles)
4. Report: “Your 24×36 claims 1 in; measured 25.37 × 25.41 mm. Camera RMS 0.6 mm.”
5. Refuse to save if centre RMS > 1.5 mm. Tell them to flatten the sheets, add light, reprint.

## Projector, same session

The white A4s are the screen. The green mat is a terrible screen.

1. Project Charuco or a ~1 s Gray-code pair.
2. Decode on the three sheets.
3. Compose `H_proj ← table`.
4. Verify with a 100 mm square on a known cell.

A Gray-code burst is worth it in v1. Eight to twenty frames, no extra hardware, soaks up cheap-projector barrel that four corners will not.

## Daily use

| Job | Scale source |
|---|---|
| Digitize | Saved lattice. Optional leftover sheet in a corner. |
| Project | Saved `H_proj`. Recheck on the exposed mat *border*, or a 30 mm fiducial strip taped to the table edge. |
| Drift | Project 4 dots, snap, nudge `H`. |

Digitize and project share one `board.json`. That is the product.
