# board.v1

Saved once per physical table. Digitize and project both read it.

```json
{
  "spec": "atelier.board/v1",
  "name": "Studio Olfa 24x36",
  "claimed": { "widthMm": 914.4, "heightMm": 609.6, "pitchMm": [25.4, 25.4] },
  "measured": {
    "pitchMm": [25.37, 25.41],
    "originPx": [120.4, 88.1],
    "axisDeg": 0.8,
    "rmsMm": { "centre": 0.6, "corners": 1.3 }
  },
  "printScale": 1.0,
  "camera": {
    "device": "optional nokhwa id",
    "size": [1920, 1080],
    "H_cam_from_table": [1, 0, 0, 0, 1, 0, 0, 0, 1]
  },
  "projector": {
    "size": [1280, 720],
    "H_proj_from_table": [1, 0, 0, 0, 1, 0, 0, 0, 1]
  },
  "createdAt": "2026-08-18T00:00:00Z"
}
```

Homographies are row-major 3×3, mapping table millimetres `(x, y, 1)` to pixel `(u, v, w)` (divide by `w`).

Refuse to write a profile with `measured.rmsMm.centre > 1.5`.
