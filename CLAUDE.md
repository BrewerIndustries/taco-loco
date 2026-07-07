# Taco Loco — notes for Claude

Mobile-first top-down taco restaurant sim (v0.1 concept). Origin: the "Taco Loco"
entry in the idea-log app (`idea-log/data/ideas.db`, id `0969698b-...`). Read that
entry for the full design (progression arc, chaos event samples, core systems).

## Architecture

- **Single file**: everything lives in `index.html` (CSS + vanilla JS, no build
  step, no deps) — same pattern as Stellar Ascent and SpaceBaseRace.
- Game state is split into `save` (money / shift / owned upgrades, persisted to
  localStorage key `tacoLoco.save.v1`) and per-shift state (orders, shelf,
  stationState, chuy, stats) reset by `startShift()`.
- Main loop: `requestAnimationFrame(loop)` with dt clamped to 100 ms.
- Input: pointer events only (works for touch + mouse). Drag-and-drop is manual —
  a fixed-position ghost element plus `document.elementsFromPoint` hit-testing on
  `pointerup` (see `startDrag`/`endDrag`).
- Stations are data-driven from the `STATIONS` map; upgrades are flags checked in
  `stationTime()` / `shelfCap()` / `patienceMax()` etc. Add new upgrades to the
  `UPGRADES` array and gate their effect with `has('id')`.

## Conventions / gotchas

- Portrait phone is the design target (max-width 480 px, `100dvh`); test with the
  mobile preview preset before calling anything done.
- `save.money` is the single wallet — earned mid-shift, spent in the shop.
  Always call `persist()` after mutating `save`.
- Ready (fully-filled) orders never expire, but their tip decays — register
  pressure is intentional.
- Chaos events: exactly one per shift, scheduled in `scheduleChaos()`, fired from
  the main loop. Interactive ones must clean up in `endShift()` (see
  `chaosActive`).
- Keep it dependency-free and single-file until the project graduates to the
  full top-down kitchen view.

## Workflow

- Git: work on `dev`; `main` only via PR the user approves (house rule).
- Update README.md after any meaningful change (house rule).
- Deployed: GitHub Pages per `jarvis-launcher/NEW-PROJECT.md` flavor A —
  main → `https://tacoloco.dabrewer.dev/`, dev → `https://tacoloco.dabrewer.dev/dev/`.
  The workflow lives on `dev` only; pushing `main` alone does not redeploy.
