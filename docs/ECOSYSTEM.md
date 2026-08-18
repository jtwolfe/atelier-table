# Atelier ecosystem

Three products, three repos, one pipeline.

```text
paper / PDF / photo
        │
        ▼
  atelier-table     ── writes garment.v1, board.v1
        │
        ▼
  atelier-studio    ── writes bake.v1 + GLBs
        │
        ▼
  atelier-looking-glass ── writes stills / clips
```

| Repo | URL |
|---|---|
| Table | https://github.com/jtwolfe/atelier-table |
| Studio | https://github.com/jtwolfe/atelier-studio |
| Looking Glass | https://github.com/jtwolfe/atelier-looking-glass |

Shared rules live in each `CONTRIBUTING.md` and `docs/STACK.md`.
`spec/garment.v1.md` is currently duplicated; extract `atelier-ir` when the first crate exists.

Seamly2D is a neighbour, not a fourth product. We consume evaluated exports. We do not fork it.
