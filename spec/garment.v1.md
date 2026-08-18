# garment.v1 — shared interchange

This file is the contract between **Table** (produces IR), **Studio** (consumes IR, produces bakes), and **Looking Glass** (consumes bakes). It is copied into each repo under `spec/`. Change it in all three at once, or extract it to a `atelier-ir` crate later.

Not a Seamly2D `.sm2d`. That format is a *construction history*. This format is *evaluated geometry* plus a sew graph.

## Units and conventions

- Millimetres.
- Right-handed, Y-up in 3D. 2D pieces live in the XY plane, +X right, +Y up, origin arbitrary per piece.
- Paths are SVG-style, but coordinates are mm, not px.
- `spec` field is required. Unknown future fields are ignored. Removing a v1 field is a breaking change.

## Schema (informative)

```json
{
  "spec": "atelier.garment/v1",
  "units": "mm",
  "source": {
    "kind": "seamly2d | digitize | svg | manual",
    "file": "optional original filename",
    "sm2dHash": "optional"
  },
  "measurements": { "bust": 920, "waist": 740, "hip": 980, "height": 1700 },
  "pieces": [
    {
      "id": "front",
      "name": "Front bodice",
      "cut": { "type": "path", "d": "M 0 0 L 100 0 L 100 200 Z" },
      "sew": { "type": "path", "d": "M 10 10 L 90 10 L 90 190 Z" },
      "grain": { "origin": [50, 20], "angleDeg": 90 },
      "notches": [{ "t": 0.42, "on": "sew", "kind": "slit" }],
      "internals": [{ "role": "dart", "d": "M …" }],
      "edges": [
        { "id": "shoulder", "from": 0.00, "to": 0.12, "lengthMm": 128.4 }
      ],
      "qty": 1,
      "fabric": "shell",
      "allowFlip": true
    }
  ],
  "stitches": [
    { "a": ["front", "shoulder"], "b": ["back", "shoulder"], "ease": 0 }
  ],
  "fabrics": {
    "shell": {
      "name": "linen",
      "weightGsm": 180,
      "stretch": [1.02, 1.15],
      "bend": 0.4,
      "friction": 0.3
    }
  },
  "layout": { "widthMm": 1500, "placements": [] }
}
```

## Rules

1. Every stitch-relevant span has a stable `id`. Studio and Table both need this.
2. `cut` and `sew` are separate closed paths. Table projects `cut`. Studio drapes `sew`.
3. Stitches are a graph, not a line colour.
4. IR is not parametric. When measurements change, re-export from Seamly2D (or re-digitize). Store `sm2dHash` so we know when a bake is stale.
5. `ease` on a stitch is mm of length difference the solver is allowed to absorb (ease / gather). `0` means the two edges should match.

## What this is not

- Not a replacement for `.sm2d` / `.smis`.
- Not a CLO `.zprj`.
- Not a GLB. The dressed mesh is `bake.v1` / `.glb`, owned by Studio.
