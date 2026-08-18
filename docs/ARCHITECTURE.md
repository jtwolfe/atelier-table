# Table — architecture

## Shape

```text
┌─────────────────────────────────────────────┐
│  laptop next to the table                   │
│                                             │
│   nokhwa ──► Table daemon (Rust / Axum)     │
│   projector display ──▲         │           │
│                       │         ▼           │
│                   board.json   garment.v1   │
│                       │                     │
│              static TS UI (embedded)        │
└───────────────┬─────────────────────────────┘
                │ HTTP + WebSocket
                ▼
     browser on same machine, or a tablet
```

The daemon owns hardware. The browser never calls `getUserMedia` for the table camera. If it did, a tablet remote-controlling the laptop would silently use the *tablet* camera and every calibration would be a lie.

## Modes

| Mode | What happens |
|---|---|
| `serve --bind 0.0.0.0:8080` | Daemon + UI. Other devices are remotes. |
| `serve --open` | Same, then open the local browser. The “desktop app.” |
| Later: Tauri window | Same API, native chrome. Optional. |

## Crates (planned)

```text
crates/
  ir/            garment.v1
  board/         board.v1, lattice fit, homography
  calsheet/      generate + detect Atelier CalSheet v1
  capture/       nokhwa frames, undistort
  project/       H_proj, Gray-code, second-display blit
  digitize/      segment, vectorize, review model
  server/        Axum + rust-embed
web/             Vite + TypeScript
```

## Pipeline

### Calibrate (once per board)

1. User prints 3 CalSheets at 100%, optionally measures the 100 mm bar or drops a credit card on one sheet.
2. Places A / B / C on the empty mat (top-left, centre, bottom-right).
3. Daemon grabs a frame, detects Charuco islands (unique ID ranges), detects the mat lattice in the gaps.
4. Joint plane fit → pitch X/Y in true mm, origin, axes, `H_cam ← table`.
5. Optional projector burst (Charuco or Gray-code onto the white A4s) → `H_proj ← table`.
6. Verify: project a 100 mm square, measure it, show residual heatmap.
7. Write `board.json`.

### Digitize

1. Pieces on the mat. Grid visible around them. Optional leftover CalSheet in a corner.
2. Warp to table-mm via `H_cam`.
3. Segment (colour gate + contours in v1).
4. Vectorize (RDP → cubic fit → close).
5. Review UI: photo under, vectors over. User names pieces, marks grain, assigns stitch edge ids.
6. Write `garment.v1`.

### Project

1. Load IR or SVG/PDF + `board.json`.
2. Layout (use Seamly nest if present, else a simple NFP later).
3. Warp through `H_proj`, fullscreen on the projector display.
4. Tools: invert, grain, notches, piece-by-piece, 1-click border re-verify.

## Why the mat, not four dragged corners

[Pattern Projector](https://www.patternprojector.com) already does 4-corner homography by eye. It is excellent and we will wrap its PDF UX ideas. Our increment is **closed-loop metric**: the mat lattice plus three known-metric islands, residual in mm, digitize and project on the same profile.

## Seamly2D

We do not link against Seamly2D (GPLv3). We read its SVG/DXF export and, if they accept it, a JSON dump of evaluated pieces. A background-image PR to Seamly is welcome; it is not this repo.
