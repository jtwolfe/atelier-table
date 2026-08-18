# Contributing

These three repos are siblings. Keep the contracts intact.

| Repo | May write | May not grow |
|---|---|---|
| [atelier-table](https://github.com/jtwolfe/atelier-table) | `garment.v1`, `board.v1` | A solver, a pose estimator |
| [atelier-studio](https://github.com/jtwolfe/atelier-studio) | `bake.v1` / GLB | A CalSheet detector, `getUserMedia` |
| [atelier-looking-glass](https://github.com/jtwolfe/atelier-looking-glass) | Stills / clips | A cloth solver, IR authoring |

`spec/garment.v1.md` is copied in all three. Change it everywhere in the same change, or extract `atelier-ir` first.

## Stack

Rust for the domain. TypeScript + Vite for the UI. One Axum binary that either binds `0.0.0.0` (remote UI) or `--open`s the local browser. See `docs/STACK.md`.

Exceptions: MediaPipe stays WASM in Looking Glass. Blender may be an optional Studio subprocess.

## Language

Code, comments, and commit messages in English. User-facing strings go through a message table from day one (Looking Glass will be embedded on shops).

## License

Apache-2.0. Do not copy GPLv3 Seamly2D sources into these trees. Reading a `.sm2d` or SVG *export* is fine.
