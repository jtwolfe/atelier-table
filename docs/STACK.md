# Stack — all three Atelier products

Dead simple for the person at the table. Fast to build. One language for the core.

## Decision

| Layer | Choice | Why |
|---|---|---|
| Domain, I/O, jobs, hardware | **Rust** | Speed, one binary, no Python deploy story |
| UI | **TypeScript + Vite** | Browser is the remote-control surface |
| HTTP | **Axum** | Serves the UI and a small JSON/WebSocket API |
| Desktop feel | Same binary, `--open` launches the system browser | Tauri later if we need an OS window |
| License | Apache-2.0 | Not a Seamly2D derivative. May *read* `.sm2d` exports / SVG. |

## Two ways to run, one binary

```text
atelier-<product> serve --bind 0.0.0.0:8080
    Headless. Any browser on the LAN is the UI.
    The machine that runs the binary owns hardware (Table)
    or the GPU job (Studio).

atelier-<product> serve --open
    Same server, then opens http://127.0.0.1:8080
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

## Exceptions (do not rewrite these in Rust)

- **MediaPipe Pose / segmenter** in Looking Glass stays WASM in the browser. The models already run there. A Rust reimplementation is a year of work for no user-visible gain.
- **Optional Blender bake backend** in Studio v0 is a *subprocess*. Our code is still Rust; Blender is an engine we may call, like ffmpeg.

## End-user bar

- One download. Double-click or `./atelier-table serve --open`.
- First screen is a verb, not a settings page: *Calibrate board* / *Bake this pattern* / *Take a photo*.
- No account. No cloud required. Looking Glass may fetch a GLB from a URL the shop already hosts.
- If something needs a number (print scale, height), ask for one number, with a picture of what to measure.
