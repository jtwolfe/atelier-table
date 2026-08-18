# Stack — Atelier products

Dead simple for the person at the table. Fast to build. One language for the core.

## Decision

| Layer | Choice | Why |
|---|---|---|
| Domain, I/O, jobs, hardware | **Rust** | Speed, one binary, no Python deploy story |
| UI (and Table web v0) | **TypeScript + Vite** | Browser is the remote-control surface; v0 is the whole loop |
| HTTP | **Axum** | Serves the UI and a small JSON/WebSocket API |
| Desktop feel | Same binary, `--open` launches the system browser | Tauri later if we need an OS window |
| License | Apache-2.0 | Not a Seamly2D derivative. May *read* `.sm2d` exports / SVG. |

## Two ways to run, one binary (v1)

```text
atelier-<product> serve --bind 0.0.0.0:8080
    Headless. Any browser on the LAN is the UI.
    The machine that runs the binary owns hardware (Table)
    or the GPU job (Studio).

atelier-<product> serve --open
    Same server, then opens the local browser.
    This is the "I double-clicked an app" path.
```

There is no separate Electron codebase. The frontend is static files embedded with `rust-embed` (or equivalent) so a single executable is enough.

Tauri 2 is an optional later wrapper if we want a native window, notifications, or tighter macOS camera permissions. It must consume the same Axum API — no forked UI.

## What lives where

```text
crates/
  ir/          garment.v1 parse/validate (shared, copy or git submodule at first)
  server/      Axum, static, websocket
  <domain>/    product-specific Rust
web/
  src/         TypeScript UI
  dist/        built assets, embedded into the binary
```

Table web v0 currently lives in the App Builder workspace, not under `web/` here. Contracts in this repo are the source of truth.

## Exceptions (do not rewrite these in Rust)

- **MediaPipe Pose / segmenter** in Looking Glass stays WASM in the browser.
- **Optional Blender bake backend** in Studio v0 is a *subprocess*.
- **Table web v0 camera** is `getUserMedia` until the daemon exists. Draft does not call a table camera.

## End-user bar

- First screen is a verb, not a settings page: *Calibrate board* / *Bake this pattern* / *Take a photo* / *New draft*.
- If something needs a number (print scale, height), ask for one number, with a picture of what to measure.
- No cloud required for the table loop. Sign-in is only for a library.
