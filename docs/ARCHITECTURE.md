# Table — architecture

## Two shapes

**Web v0 (now)** — the browser *is* the app. Camera via `getUserMedia` stills. Projector via a second tab (`/output`) synced with `BroadcastChannel` + `localStorage`. See [WEB.md](WEB.md).

**Daemon v1 (destination)** — the laptop next to the table owns hardware. The browser is a remote. That is the shape below.

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

The daemon owns hardware. In v1 the browser never calls `getUserMedia` for the table camera. If it did, a tablet remote-controlling the laptop would silently use the *tablet* camera and every calibration would be a lie.

Web v0 breaks that rule on purpose: there is no daemon yet, and the laptop *is* the table machine.

## Modes

| Mode | What happens |
|---|---|
| Web v0 | Vite / TanStack app. Device camera. Second tab = projector. |
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

**v0:** print 3 CalSheets, photograph, click four mat corners + 100 mm bar. Homography + print scale. Optional autocorrelation of the visible lattice. Sheets come off; the lamp draws the grid.

**v1:** daemon frame → detect Charuco islands (unique ID ranges) + mat lattice → joint plane fit → optional Gray-code projector burst → `H_proj`. Refuse a board with centre RMS > 1.5 mm.

### Digitize

1. Pieces on the mat. Grid visible around them (projected, or the printed mat).
2. Warp to table-mm via `H_cam`.
3. Segment (colour gate + contours in v0/v1).
4. Vectorize (RDP → close).
5. Review UI: photo under, vectors over. User names pieces, marks grain.
6. Write `garment.v1`. Optionally emit evaluated `.sm2d`.

### Project

1. Load IR or SVG + `board.json`.
2. **v0:** lamp in this tab or `/output`. Four dragged corners for keystone. Board grid from measured pitch.
3. **v1:** warp through `H_proj`, fullscreen on the projector display. Gray-code re-verify.

## Why the mat, not four dragged corners

[Pattern Projector](https://www.patternprojector.com) already does 4-corner homography by eye. It is excellent and we will wrap its PDF UX ideas. Our increment is **closed-loop metric**: the mat lattice plus three known-metric islands, residual in mm, digitize and project on the same profile.

## Seamly2D and Draft

We do not link against Seamly2D (GPLv3). We read SVG/DXF and write evaluated `.sm2d` (points + pieces, no live increments). Live formulas belong in [Atelier Draft](https://github.com/jtwolfe/atelier-draft).
