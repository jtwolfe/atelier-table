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
- Checker 5×7 @ 28 mm (Charuco IDs in v1; v0 is a printed checker plus a huge letter).
- 100.00 mm bar. 50 mm square. Credit-card ghost (85.60 × 53.98 mm).
- Huge “PRINT AT 100% / DO NOT SCALE”.

Download the three SVGs or open the print layout. Print at 100%. Do not fit to page.

## Print-scale check (pick one)

1. User types what a ruler says the 100 mm bar measures. Best.
2. Photograph a credit card in the ghost box. Good.
3. **v0:** click both ends of the 100 mm bar on the photo. That *is* the check.
4. “I printed at 100% and I trust it.” Acceptable (skip the bar).

That number `s` applies to all three sheets from that print batch. In v0, `printScale = measuredBarMm / 100`, and claimed pitch is divided by it.

## Placement

On a 24×36″ mat: A top-left, C centre, B bottom-right. Do not overlap, do not hang off the edge. Alignment to the grid is nice, not required.

On 18×24″: same idea, tucked in.

## Solve

### v0 (shipping)

1. Click the four outer mat corners → `H_cam ← table`.
2. Click the 100 mm bar → print scale.
3. Optional: sample the photo along table axes, autocorrelate, keep pitch if it is within 25% of the bar.
4. Report measured pitch and corner RMS in millimetres.
5. **Take the CalSheets off.** Project draws the board grid from `measured.pitchMm`.

### v1 (daemon)

1. Detect Charuco per sheet → three `paper-mm → camera` homographies, scaled by `s`.
2. Detect the mat lattice in the gaps (LSD / constrained Hough). Two vanishing points. Drop 45° bias lines — they fail the “two-family axis-aligned lattice” test.
3. Everything lives on one plane. Fit origin, axis angle, pitch X/Y, a small affine residual.
4. Report: “Your 24×36 claims 1 in; measured 25.37 × 25.41 mm. Camera RMS 0.6 mm.”
5. Refuse to save if centre RMS > 1.5 mm.

## Projector, same session

**v0:** open `/output` as a second tab. Fullscreen it on the projector. Drag the four chalk corners to keystone. The control page and the lamp stay linked.

**v1:** the white A4s are the screen during calibrate. Project Charuco or a ~1 s Gray-code pair, compose `H_proj ← table`, verify a 100 mm square.

## Daily use

| Job | Scale source |
|---|---|
| Digitize | Saved lattice / `H_cam`. Sheets already off. |
| Project | Saved board + session keystone (v0) or `H_proj` (v1). The lamp grid is the alignment. |
| Drift | Nudge the four chalk corners, or (v1) project 4 dots, snap, nudge `H`. |

Digitize and project share one `board.json`. That is the product.
