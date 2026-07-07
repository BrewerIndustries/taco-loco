# 🌮 Taco Loco

Mobile-first top-down taco restaurant sim — from line cook to taco empire.
Built from the [Idea Log](https://github.com/BrewerIndustries/idea-log) entry "Taco Loco".
Same cosy-chaotic energy as the Chaos series, but greasier.

## Play

Single-file game — open `index.html` in any browser, or serve it statically:

```bash
python3 -m http.server 4977 --directory .
```

Designed for **portrait phone** (touch), works fine with a mouse on desktop.

## v0.1 — what's in the build

Implements the v0.1 design from the idea log:

- **4 stations** — Grill 🔥 (tacos), Assembly 🫔 (burritos), Toppings 🥑 (nachos),
  Register 💰. Tap a station to work it; a progress bar fills and the item pops
  onto the pickup shelf. You can only work one station at a time.
- **Order cards** — slide in on the left of the queue, each showing a customer,
  required items, dollar value, and a draining patience bar. Run out of patience
  and the customer storms out.
- **Drag to serve** — drag finished food from the pickup shelf onto a matching
  order card. When all items are filled the card flips to **READY** — tap the
  register to ring it up. Faster service = bigger tip.
- **Staff hire (1)** — Chuy 🧑‍🍳, purchasable in the shop. Specialty: grill
  (1.4× speed there, 0.75× elsewhere). Quirk: randomly naps for 4 s after
  finishing an item. Auto-works the highest-demand station and rings up ready
  orders; tap his chip then tap a station to direct him, tap again for auto.
- **Shift loop** — 3-minute shifts with a ramping spawn rate; end screen shows
  orders served, sales, tips, and a 🌶️ chaos rating (driven by walkouts and
  botched chaos events).
- **Upgrade shop** — 3 random picks between shifts, buy one or skip: Hotter
  Grill, Prep Drills, Comfy Seating, Auto-Toppings, Hire Chuy, Bigger Shelf,
  Loyalty Cards, Salsa Insurance.
- **Chaos events** — one random event per shift: 🫙 *Salsa Explosion* (tap all
  the splats in 7 s or everyone's patience tanks), 📸 *Influencer* (next order
  rung up pays triple), 🚌 *Tour Bus* (3 orders at once).
- **Persistence** — money, shift number, and owned upgrades saved to
  `localStorage` (`tacoLoco.save.v1`); reset button on the title screen.

## Tech

Vanilla JS + CSS in one `index.html` — no build step, no dependencies. Pointer
events for unified touch/mouse tap + drag, `requestAnimationFrame` loop with a
clamped delta so tab-switching doesn't nuke a shift. Same single-file pattern as
Stellar Ascent / Space Base Race, so it drops straight into the GitHub Pages
dev/main pipeline.

## Deploy (not yet done)

Follow `jarvis-launcher/NEW-PROJECT.md`, flavor **A) Static site → GitHub Pages**:

1. Create `BrewerIndustries/taco-loco` with `main` + `dev` branches.
2. Add `.github/workflows/pages.yml` on `dev` (main → `/`, dev → `/dev/`,
   CNAME `taco-loco.dabrewer.dev`); allow `dev` in the `github-pages` environment.
3. Cloudflare CNAME `taco-loco.dabrewer.dev` → `brewerindustries.github.io` (DNS-only).
4. Register in `jarvis-dashboard/scripts/sync-registry.mjs` (REPOS + DEFAULTS)
   and add a launcher tile in `jarvis-launcher` `src/lib/apps.ts`.

## Backlog / next steps (from the idea-log full design)

- Real top-down kitchen view with walkable staff sprites (replace the station grid)
- More staff with specialty + disaster-trait combos; staff roster screen
- Order customisations ("extra everything") worth more but punishing to flub
- Menu R&D: new taco types, sauces, seasonal specials
- Second location / multi-location map view
- Reputation & reviews; food critic encounters
- Sound effects + music
- More chaos events (salsa heat contest, tortilla shortage, cashiers defecting)
