# Atelier ecosystem

Four products, four repos, one pipeline.

```text
  atelier-draft     -- writes draft.v1 (live) and garment.v1 (evaluated)
        |
        v
paper / photo
        |
        v
  atelier-table     -- writes garment.v1, board.v1
        |
        v
  atelier-studio    -- writes bake.v1 + GLBs
        |
        v
  atelier-looking-glass -- writes stills / clips
```

| Repo | URL |
|---|---|
| Table | https://github.com/jtwolfe/atelier-table |
| Studio | https://github.com/jtwolfe/atelier-studio |
| Looking Glass | https://github.com/jtwolfe/atelier-looking-glass |
| Draft | https://github.com/jtwolfe/atelier-draft |

Shared rules live in each `CONTRIBUTING.md` and `docs/STACK.md`.
`spec/garment.v1.md` is currently duplicated; extract `atelier-ir` when the first crate exists.

Seamly2D is a neighbour, not a fifth product. We consume evaluated exports. We do not fork it. Live formulas live in Draft.
