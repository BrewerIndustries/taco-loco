# 🌮 Taco Loco

A cozy, top-down **Overcooked-style** taco-kitchen game. Walk your little chef
around the kitchen: grab ingredients, grill and chop, plate them up, and run the
order to the serving window before the customer's patience runs out. Cuter,
cleaner and calmer than Overcooked — soft palette, rounded tiles, a hand-drawn
chef. Born from the [Idea Log](https://github.com/BrewerIndustries/idea-log)
entry "Taco Loco".

## Play

Single-file game — open `index.html` in any browser, or serve it statically:

```bash
python3 -m http.server 4977 --directory .
```

**Controls are tap / click** (touch on phone, mouse on desktop): tap a spot to
walk there, tap a **station** to walk up and automatically use it (grab, drop,
cook, chop, plate, serve). The chef pathfinds around obstacles. WASD / arrows
still work as an optional desktop fallback.

**Clarity built in:** every station is labelled, and whatever you're holding makes
its valid destinations **glow green** (carry raw meat → the grills glow; carry a
finished dish → the serve window glows), so the next move is always obvious. A
first-time **interactive tutorial** on Taco Truck level 1 walks through a whole
taco step by step (orders + timer pause until you finish).

The kitchen renders in an angled **2.5D perspective** — a foreshortened floor with
tall, depth-sorted counters (the chef passes correctly in front of and behind
them), front faces, labels and soft contact shadows, for an Overcooked-style look.

## How it plays

- **Move** the chef with the left stick (or WASD). You carry **one item** at a time.
- **USE** is context-sensitive against whatever station you're facing (a yellow
  outline shows your target):
  - **Crate** → grab a fresh raw ingredient
  - **Grill 🔥** → drop raw meat/fish to cook; it cooks on its own, but leave it
    too long and it **burns** (bin it and start over)
  - **Board 🔪** → drop whole lettuce/jalapeño; it chops itself over a few seconds
  - **Plate 🍽️** (assembly) → add prepped components; when they match a recipe on
    the menu the finished dish appears — pick it up
  - **Window 🔔** → deliver a finished dish to fill a matching order
  - **Bin 🗑️** → trash a mistake · plain **counters** stage items you're juggling
- **Orders** stream in as tickets up top, each with a draining patience bar.
  Deliver faster for a bigger bonus; let one expire and you lose points.
- **Stars** at the end (1–3) from your score; you need ≥1 star to unlock the next
  level. Progress is saved to `localStorage` (`tacoLoco.save.v2`).

## Content

**🚚 Taco Truck** — 5 levels, the tutorial location. Ramps from a simple
2-ingredient taco up through lettuce-chopping, parallel **Nachos**, **Burritos**,
and a full three-dish lunch rush.

**🏖️ Beach Cantina** — unlocks after clearing the Taco Truck. Its **twist**:
the sandy floor is **slippery** (the chef drifts), a **prep island** blocks the
middle of the kitchen, and beach-goers are more impatient. Adds **fish 🐟**
(grilled) and **salsa 🥫** with new dishes — **Fish Tacos**, **Quesadillas** and
**Loaded Nachos**.

New locations are pure data (`LOCATIONS` array) — a location is an emoji, a blurb,
a set of modifiers (`slippery`, `patienceScale`), and a list of levels (layout +
recipes + timing + star thresholds), so more kitchens/twists drop in easily.

## Tech

Vanilla JS + CSS in one `index.html` — no build step, no dependencies. The
kitchen is **Canvas 2D** (grid-based tiles, per-axis circle/tile collision, a
hand-drawn chef and stations, emoji ingredient glyphs); the HUD, order tickets,
map and overlays are DOM. A single `requestAnimationFrame` loop with a clamped
delta drives everything, plus tiny WebAudio blips for feedback. Same single-file
pattern as Stellar Ascent / Space Base Race, so it drops straight into the GitHub
Pages dev/main pipeline.

## Deploy

GitHub Pages via `.github/workflows/pages.yml` (lives on `dev`, per
`jarvis-launcher/NEW-PROJECT.md` flavor A):

- **prod** `https://tacoloco.dabrewer.dev/` ← `main` branch
- **dev** `https://tacoloco.dabrewer.dev/dev/` ← `dev` branch

Pushing to `dev` redeploys both paths. Pushing to `main` alone does **not**
redeploy — re-run the workflow (or push to dev) after a promotion PR merges.
DNS: Cloudflare CNAME `tacoloco` → `brewerindustries.github.io` (DNS-only).
Registered in the dashboard registry (`jarvis-dashboard/scripts/sync-registry.mjs`)
and the launcher (`jarvis-launcher/src/lib/apps.ts`).

Workflow: commit to `dev`; promote to `main` only via an approved PR.

## Backlog / next steps

- More locations & twists (conveyor belts, dirty-plate washing, moving hazards)
- Co-op / second chef; local two-thumb or two-player
- Order customisations ("extra everything") worth more but punishing to flub
- Menu R&D: new taco types, sauces, seasonal specials
- Combo/tip streaks and a per-level score chase (beat your best)
- Music + richer SFX; screen-shake and juicier feedback
- Animated customers at the window; reputation / food-critic encounters
