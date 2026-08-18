# Atelier Table

Digitize paper patterns and project layouts onto fabric.

The cutting mat is the ruler. Three printed A4 CalSheets teach the software what that ruler actually measures. A webcam and an optional cheap projector do the rest.

This is **product 1** of the Atelier trio.

| Product | Repo | Role |
|---|---|---|
| **Table** | this repo | Paper ↔ cloth, in millimetres |
| Studio | [atelier-studio](https://github.com/jtwolfe/atelier-studio) | Offline drape and render |
| Looking Glass | [atelier-looking-glass](https://github.com/jtwolfe/atelier-looking-glass) | Photo / short-clip try-on |

Table **produces** `garment.v1` IR. It does not simulate cloth and it does not try clothes on a person.

## Who it is for

You, standing at a cutting table. You have a self-healing mat, a laptop, a USB webcam, and maybe a mini projector. You do not want to tape A0 PDFs together, and you do not want to re-digitize a traced block by clicking a hundred points.

## What v1 does

1. **Calibrate the board** — print 3 CalSheets, put them on the empty mat, take one photo. We measure the real grid pitch (cheap mats lie) and save `board.json`.
2. **Digitize** — drop pattern pieces on that mat, snap, review vectors, export IR.
3. **Project** — load IR (or SVG/PDF), warp through the saved projector homography, cut on the light.

## How you run it

Rust binary, TypeScript UI, one process.

```bash
# on the laptop next to the table (owns the webcam + projector)
atelier-table serve --bind 0.0.0.0:8080 --open
```

Open the same URL from a tablet on the bench if you want a bigger touch target. The **camera still belongs to the laptop** — the UI is only a remote control. See [docs/ARCHITECTURE.md](docs/ARCHITECTURE.md).

## Status

Documentation and contracts. No application code yet.

- [Goals](docs/GOALS.md)
- [Architecture](docs/ARCHITECTURE.md)
- [Calibration](docs/CALIBRATION.md)
- [Research / prior art](docs/RESEARCH.md)
- [Stack](docs/STACK.md)
- [garment.v1](spec/garment.v1.md)
- [board.v1](spec/board.v1.md)
- [CalSheet v1](spec/calsheet.v1.md)

Apache-2.0. Not a fork of [Seamly2D](https://github.com/FashionFreedom/Seamly2D); we speak to it through SVG/DXF/IR.
