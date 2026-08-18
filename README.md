# Atelier Table

Digitize paper patterns and project layouts onto fabric.

The cutting mat is the ruler. Three printed A4 CalSheets teach the software what that ruler actually measures. A webcam and an optional cheap projector do the rest.

This is **product 1** of Atelier.

| Product | Repo | Role |
|---|---|---|
| **Table** | this repo | Paper ↔ cloth, in millimetres |
| Studio | [atelier-studio](https://github.com/jtwolfe/atelier-studio) | Offline drape and render |
| Looking Glass | [atelier-looking-glass](https://github.com/jtwolfe/atelier-looking-glass) | Photo / short-clip try-on |
| Draft | [atelier-draft](https://github.com/jtwolfe/atelier-draft) | Live CAD + libraries + Commons |

Table **produces** `garment.v1` IR and an evaluated `.sm2d`. It does not simulate cloth, try clothes on a person, or own live formulas. That last job is Draft.

## Who it is for

You, standing at a cutting table. You have a self-healing mat, a laptop, a USB webcam, and maybe a mini projector. You do not want to tape A0 PDFs together, and you do not want to re-digitize a traced block by clicking a hundred points.

## What you can do now (web v0)

A working browser loop exists (TanStack / TypeScript). It is the app you can use today:

1. **Print CalSheets** — A, B, C as real SVG files, plus a print layout. 100%. Do not fit to page.
2. **Calibrate** — photograph the empty mat (upload or device camera). Click four corners and the 100 mm bar. Optional: read grid pitch from the photo lattice. Residual in millimetres. Save `board.v1`.
3. **Sheets come off** — the lamp can draw the board grid from the measured pitch. You do not leave CalSheets on the fabric.
4. **Digitize** — white paper on the mat, trace or click a contour, or import SVG / `garment.v1`. Export JSON, SVG, or evaluated `.sm2d`.
5. **Project** — pieces on the photo, or lamp mode. Second window/tab is the projector output (same session). Drag, rotate, flip, keystone.

The long-run product is still a **Rust daemon** that owns the webcam and the projector display, with the browser as a remote. See [docs/ARCHITECTURE.md](docs/ARCHITECTURE.md) and [docs/WEB.md](docs/WEB.md).

## How you run the long-run binary (not built yet)

```bash
# on the laptop next to the table (owns the webcam + projector)
atelier-table serve --bind 0.0.0.0:8080 --open
```

## Status

| Layer | State |
|---|---|
| Contracts | `board.v1`, `garment.v1`, CalSheet v1 |
| Web v0 | Working loop in the App Builder preview (calibrate → digitize → project). Source not vendored here yet — this repo stays the product home and the spec. |
| Rust daemon | Not started. Still the target for hardware ownership. |

- [Goals](docs/GOALS.md)
- [Architecture](docs/ARCHITECTURE.md)
- [Web v0](docs/WEB.md)
- [Calibration](docs/CALIBRATION.md)
- [Research / prior art](docs/RESEARCH.md)
- [Stack](docs/STACK.md)
- [Ecosystem](docs/ECOSYSTEM.md)
- [garment.v1](spec/garment.v1.md)
- [board.v1](spec/board.v1.md)
- [CalSheet v1](spec/calsheet.v1.md)

Apache-2.0. Not a fork of [Seamly2D](https://github.com/FashionFreedom/Seamly2D); we speak to it through SVG / evaluated `.sm2d` / IR.
