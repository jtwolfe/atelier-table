# Table — goals

## Job

Make the cutting table a calibrated instrument, then use it in two directions:

- **In:** paper / photo / PDF → `garment.v1`
- **Out:** `garment.v1` (or a layout) → light on fabric, in true millimetres

## Non-goals

- 3D drape, avatars, try-on (Studio / Looking Glass).
- Becoming a parametric CAD program (that is Seamly2D).
- Driving a laser or CNC. Projection is for a human with a rotary cutter.
- “Any casual photo of tissue on the couch.” v1 assumes a mat in frame.

## User promises

1. One empty-board photo and the mat is metric, even if the printed 1″ is 25.37 mm.
2. Digitize and project share that same `board.json`. You calibrate once per table.
3. The first useful digitize path is *white paper on a green/grey mat*, not vintage tissue.
4. Residual error is shown in millimetres. We refuse to pretend a 4 mm board is calibrated.
5. A person who can print an A4 and plug in a webcam can finish calibration without reading this file.

## Success tests

| Test | Pass |
|---|---|
| Calibrate | 3 CalSheets on a 24×36″ Olfa, RMS < 1 mm centre, < 2 mm corners |
| Mat honesty | Reported pitch differs from 25.4 mm on a cheap mat, and a 100 mm projected square matches a ruler |
| Digitize | Printed Seamly shirt block recovered within 2 mm on major edges |
| Project | Projected grain line sits on a drawn grain line across 400 mm |
| Remote UI | Tablet on the bench controls the laptop; webcam is still the laptop’s |

## Product boundaries

Table may **write** IR and layouts. It may **read** IR, SVG, DXF, PDF. It may **not** grow a cloth solver or a pose estimator.
