# Atelier CalSheet v1

Printable A4 used in sets of three to bootstrap a cutting mat.

## Physical

| | |
|---|---|
| Page | A4 210 × 297 mm |
| Quiet margin | 10 mm (cheap inkjets clip edges) |
| Paper | Matte copy paper, not glossy |
| Ink | Black |

## Markers

| Element | Spec |
|---|---|
| Charuco | 5×7 squares, 28 mm, 4 mm inner margin |
| Dictionary | ArUco 4×4 |
| Sheet A IDs | 0–19 |
| Sheet B IDs | 20–39 |
| Sheet C IDs | 40–59 |
| Corner AprilTags | 32 mm, IDs 100+N … (fallback if Charuco is occluded) |
| 100.00 mm bar | Two hairlines, labelled |
| 50 mm square | Solid black (detect Y-squash from a bad print driver) |
| Credit-card ghost | 85.60 × 53.98 mm (ISO/IEC 7810 ID-1) |
| Grey / white patches | 20 mm, ~18% grey + paper white (projector exposure) |
| Label | `SHEET A` / `B` / `C` and `PRINT AT 100% · DO NOT SCALE` |

## Placement (24×36″ mat)

```
A (top-left)     exposed grid
     C (centre)
exposed grid          B (bottom-right)
```

Sheets must not overlap. They need not be grid-aligned.

## Detection contract

A detector returns, per sheet:

- `id`: `A` | `B` | `C`
- corner and Charuco points in image pixels
- corresponding points in paper-mm (true A4 coordinates, origin top-left of the page, +X right, +Y down on the page — convert to table Y-up in the board solver)

If fewer than 8 interior Charuco corners are visible on a sheet, that island is discarded. Two surviving islands are enough to finish; three is the target.
