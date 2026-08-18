# Table — research and prior art

What already exists, what we steal, what we refuse to rebuild.

## Projection onto fabric

### Pattern Projector
- [patternprojector.com](https://www.patternprojector.com) · [GitHub](https://github.com/Pattern-Projector/pattern-projector) · MIT · Next.js / TypeScript · ~220★
- Calibrates by dragging four corners of a projected grid onto a physical mat. User types mat width/height.
- Loads PDF, invert, flip, rotate, fullscreen. Offline-capable. Weblate i18n.
- **Steal:** PDF UX, invert-for-visibility, the “just project the damn pattern” tone.
- **Do not steal:** 4-corner-by-eye as the only calibrator. No camera, no residual in mm, no digitize path, no shared board profile.

### PDFStitcher
- Open-source tiled-PDF assembly, well known in the projector-sewing community.
- **Steal:** tiled-PDF ideas if we ingest commercial patterns. Not a calibrator.

### Project & Cut / commercial projector-sewing apps
- Same 4-corner model, paid PDF features, layer toggles for size lines.
- Ceiling, not a codebase.

### Industrial (Gerber, Lectra, Audaces Digiflash)
- Calibrated tables, overhead cameras, photo-to-piece in one click, sometimes projection + cutter.
- Audaces Digiflash is the closest “photo a paper pattern, get CAD.” Closed, expensive, Windows.
- We are a poor person’s version of that loop, on a $30 mat and a $20 webcam.

## Camera–projector geometry

| Technique | Use |
|---|---|
| Planar homography (`findHomography`) | v1. Flat table. 4+ correspondences. |
| Charuco / AprilTag | Dense, robust correspondences on CalSheets. |
| Gray-code structured light | Dense `cam ↔ proj` map. Barrel, leftover-cloth later. |
| Full procam intrinsics + stereo | Overkill for a flat table. Defer. |

Useful references: OpenCV homography tutorial; [BingyaoHuang/single-shot-pro-cam-calib](https://github.com/BingyaoHuang/single-shot-pro-cam-calib) (research, non-commercial license — read, don’t copy); any MIT Gray-code decoder.

Rust: `opencv` crate if we accept the native dep; otherwise `image` + a small AprilTag/Charuco port or `apriltag` crate. Webcam: [`nokhwa`](https://github.com/l1npengtul/nokhwa). Do **not** put the table camera in the browser.

## Digitization

The home-sewist status quo is: photograph with a 2″ square, trace in Inkscape or Illustrator. Industry uses large-format scanners or Digiflash-class photo CAD.

Classical CV is enough for v1 on *white paper on a contrasting mat*:

1. Warp to table-mm (we already have `H_cam`).
2. Colour gate / adaptive threshold.
3. Contours, reject the grid (axis-aligned lattice).
4. Ramer–Douglas–Peucker, cubic fit, close.

`vtracer` / `potrace` are fine on clean scans and will happily trace shadows on photos — gate them behind segmentation.

v2 (not this repo’s first milestone): SAM 2 / YOLO-seg trained on synthetic “Seamly SVG dropped on photographed mats.” Always keep a lasso corrector. Automatic is a proposal.

**Do not** promise photo → live `.sm2d` construction history. That is inverse CAD (NeuralTailor / SewFormer). Dead IR that a human can rebuild in Seamly is the win.

## Seamly2D touchpoints

- [FashionFreedom/Seamly2D](https://github.com/FashionFreedom/Seamly2D) — GPLv3+, Qt 6, parametric. No plugin host.
- They have said DXF import is the wrong shape for a formula-driven file.
- Open issues: [#440](https://github.com/FashionFreedom/Seamly2D/issues/440) image-as-background, [#32](https://github.com/FashionFreedom/Seamly2D/issues/32) 3D body scan.
- Forum: `.sm2d` is a history; only `point type="single"` has absolute coordinates. External tools should consume *evaluated* geometry.

We send small PRs upstream (background image, JSON dump). We do not fork.

## What Table will not become

A Pattern Projector clone with extra buttons. If all we ship is 4-corner PDF projection, we should have contributed to Pattern Projector instead.
