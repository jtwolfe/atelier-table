# Table — web v0

What the running browser app actually does. The daemon in [ARCHITECTURE.md](ARCHITECTURE.md) is still the destination. This file is the honesty check.

## Surfaces

| Route | Job |
|---|---|
| `/` | Three-step landing. Studio sample loads a 24×36 board + shirt block. |
| `/calsheet` | Print / download A, B, C. Real files, not a blocked blob click. |
| `/calsheet/print` | Clean A4 print layout. 100%. Do not scale. |
| `/calibrate` | Photo or device camera. Four mat corners, 100 mm bar, optional lattice pitch. |
| `/digitize` | Trace / click contour / import SVG or JSON. Export `garment.v1`, SVG, evaluated `.sm2d`. |
| `/project` | Pieces on the photo, or lamp. Board grid. Keystone handles. |
| `/output` | Second tab. Fullscreen projector. Same session via `BroadcastChannel` + `localStorage`. |
| `/library` | Session board + garment. Signed-in store. `.sm2d` download. |

## What is metric

- Homography from four clicked mat corners (table mm → camera px).
- Print scale from the 100 mm bar (`printScale = measuredBar / 100`).
- Claimed pitch is divided by print scale so a cheap mat that is not 25.40 mm is reported honestly.
- After commit, a strip of the photo is autocorrelated for lattice pitch. If it agrees within 25% of the bar, we keep the detected pitch.
- Lamp grid is drawn from `board.measured.pitchMm`. CalSheets can leave the table.

## What is not the daemon yet

- The **browser** owns the camera (`getUserMedia` stills). Fine when the laptop *is* the table machine. A tablet remote would see the *tablet* camera — that is why v1 still wants a daemon.
- Projector output is a **second browser tab**, not a second OS display. Fullscreen that tab on the projector input.
- Session lives in `sessionStorage`. The lamp window is linked through `localStorage` key `atelier-table-output` and channel `atelier-table-v1`.
- `.sm2d` is **evaluated** (single points + piece node lists). Rebuild formulas in Draft or Seamly.

## IR written

- `atelier.board/v1` — claimed size, measured pitch, print scale, `H_cam_from_table`.
- `atelier.garment/v1` — closed paths in millimetres, grain, edges.

No `H_proj` Gray-code yet. Lamp keystone is four dragged corners on the output, per session, not saved on the board.
