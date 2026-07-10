# 🌮 Taco Loco — Food Truck Rush

A cozy, fast **food-truck window** game. You're inside your taco truck looking out
the service window; customers show up with orders and you **tap the ingredients on
your prep counter in the correct sequence** to build each one. No walking — pure
tap/click gameplay.

> This is a fresh concept. The earlier top-down / Overcooked-style walking version
> is preserved on the **`concept/overcooked-walk`** branch.

## Play

Single-file game — open `index.html` in any browser, or serve it statically:

```bash
python3 -m http.server 4977 --directory .
```

Designed for **portrait phone** (tap), works with a mouse on desktop.

## How it plays

- Customers appear at the **window** with order tickets. Each ticket shows the dish
  and the **exact sequence** of ingredients as icons, plus a patience bar.
- Build the **active** order (highlighted) by tapping ingredient buttons on the prep
  counter **in the order shown**. Tap another ticket to switch which order you work.
- The **next ingredient glows** — on the ticket and on the counter button — so the
  right move is always obvious.
- A **wrong** ingredient is a small penalty: it breaks your combo and nicks the
  customer's patience, but your progress isn't wiped.
- Finish orders fast and clean for **tips** and a **combo multiplier** (x1.3 → x2).
  Let a customer's patience run out and they leave hungry (score penalty).
- A shift lasts ~100s with orders arriving faster and new recipes unlocking; the end
  screen scores you 1–3 ⭐. Recipes: Taco, Quesadilla, Nachos, Burrito, Bowl.

## Tech

Vanilla JS + CSS in one `index.html` — no build step, no dependencies. Pure DOM UI
(order tickets, ingredient buttons) with CSS animations for juice, a
`requestAnimationFrame` loop for the timer/patience/spawning, and tiny WebAudio
blips. Best-score + shift count persist to `localStorage` (`tacoLoco.truck.v1`).
Same single-file pattern as the other Jarvis games, so it drops straight into the
GitHub Pages dev/main pipeline.

## Deploy

GitHub Pages via `.github/workflows/pages.yml` (lives on `dev`): **main → `/`**
(prod, `https://tacoloco.dabrewer.dev/`), **dev → `/dev/`** (dev,
`https://tacoloco.dabrewer.dev/dev/`). Pushing `main` alone does not redeploy —
re-run the workflow or push `dev`.

## Backlog / ideas

- Multiple trucks / locations with distinct menus and a level map
- Prep steps that need a station tap (grill/chop) before an ingredient is "ready"
- Hide the sequence on later levels for a memory challenge
- Power-ups (freeze patience, auto-prep), day/night, weather
- Music + richer SFX; happy/angry customer reactions at the window
