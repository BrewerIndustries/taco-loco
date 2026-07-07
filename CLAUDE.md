# Taco Loco — notes for Claude

A cozy, top-down **Overcooked-style** taco-kitchen game (walk the chef, grab/cook/
chop/plate/serve). Origin: the "Taco Loco" entry in the idea-log app
(`idea-log/data/ideas.db`, id `0969698b-...`) — that entry describes an earlier
tap-the-station sim; the current game is the Overcooked-style redesign the user
asked for (top-down movement, 5 levels then a new location with a unique twist,
cuter/cozier art). Read the idea-log entry only for flavour/backlog, not the
mechanics.

## Architecture

- **Single file**: everything lives in `index.html` (CSS + vanilla JS, no build
  step, no deps) — same pattern as Stellar Ascent and SpaceBaseRace.
- **Render split**: the kitchen is **Canvas 2D** (`#game`); the HUD, order
  tickets, world map and overlays are DOM. One `requestAnimationFrame` `frame()`
  loop with dt clamped to 50 ms drives `update()` + `render()` when
  `screen === 'play'`.
- **Content is data**: `LOCATIONS[]` → each has `mod` (e.g. `slippery`,
  `patienceScale`) and `levels[]`; a level has a `layout` (array of strings),
  `start`, `recipes`, timing, and `stars` thresholds. `ING`, `RECIPES`, `CRATE`
  and the `LAYOUT_*` grids define the rest. Add a kitchen/level/dish purely by
  editing these — no engine changes.
- **Layout chars**: `.`=floor, `#`=counter (staging), crate letters (see `CRATE`),
  `G`=grill, `B`=board, `A`=assembly/plate, `D`=deliver window, `W`=bin. Border
  cells are solid; corners must stay plain walls (stations need an interior
  neighbour to be faced).
- **Movement/collision**: player is a circle (radius `R`) in pixel space; per-axis
  `blocked()` samples the tiles under the bounding box. `slippery` swaps snappy
  velocity for eased/drifting velocity.
- **Interaction**: `frontStation()` = the tile one step in the facing direction;
  `interact()` is a switch on station type. Item model is `{base,state}` (or
  `{base:'dish',recipe}`); `isReady/canGrill/canChop` gate what each station
  accepts. Stations auto-process (`updateStations`): board chops, grill cooks →
  `cooked` → `burnt` if left.
- **Save**: `tacoLoco.save.v2` = `{stars:{'<locId>:<levelIdx>': 0-3}}`. Unlock via
  `levelUnlocked()` (next level needs ≥1 star; next location needs all prev levels
  starred). Always `persist()` after mutating `save`.
- **Debug/test hooks**: `window.TL` exposes `start(li,lv)`, `teleport(gx,gy)`,
  `face(dir)`, `use()`, `tick(dt)`, `carry()`, `front()`, `setTime()`. Use these to
  drive deterministic verification — the RAF loop is throttled when the preview is
  backgrounded, so step frames with `TL.tick()` rather than real-time waits.

## Conventions / gotchas

- Portrait phone is the design target (max-width 480 px, `100dvh`); test with the
  mobile preview preset. Canvas has a fixed internal size (`cols*TILE`) and is
  CSS-scaled to fit — game logic is in grid/pixel units, independent of display.
- Keep it dependency-free and single-file.
- **No non-ASCII in code** (a stray Devanagari digit once slipped into a hex
  colour). Emoji are fine inside strings only.

## Workflow

- Git: work on `dev`; `main` only via PR the user approves (house rule).
- Update README.md after any meaningful change (house rule).
- Deployed: GitHub Pages per `jarvis-launcher/NEW-PROJECT.md` flavor A —
  main → `https://tacoloco.dabrewer.dev/`, dev → `https://tacoloco.dabrewer.dev/dev/`.
  The workflow lives on `dev` only; pushing `main` alone does not redeploy.
