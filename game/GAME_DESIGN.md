# Abagold Abalone Farm — Game Design Notes

A mobile-first farming/idle game celebrating abalone (perlemoen) farming in Hermanus,
South Africa. The goal: be genuinely fun and sticky, teach players about abalone
farming, and make them hungry enough to buy and eat the real thing.

Play it at: `/game/` on the deployed site. Installable as an app (PWA) from the
browser menu ("Add to Home Screen").

## Research: what makes farming/idle games addictive

Mechanics borrowed from FarmVille, Hay Day, and the idle-game genre, and how this
game implements each:

| Proven mechanic | Where it comes from | In this game |
|---|---|---|
| **Appointment timers** — plant now, return later | FarmVille/Hay Day core loop | Abalone grow through 5 real-time stages (Spat → Juvenile → Medium → Large → Jumbo) |
| **Soft "wither"** — neglect has a cost, pulling players back | FarmVille's withering crops (softened — no cruelty) | Feed and water-quality meters decay; hungry abalone in dirty tanks grow up to ~8× slower |
| **Offline/idle progress with a cap** | Idle games (Egg Inc., AdVenture Capitalist) | Farm keeps running up to 8h while away; "Welcome back" summary modal shows what happened |
| **Variable rewards** | Slot-machine psychology used across F2P | Random events: pearl finds, free kelp deliveries, pump breakdowns, seal visits, surprise export orders |
| **Urgency windows** | Hay Day boat orders | Export Order event: +60% prices for 90 seconds — sell now! |
| **Daily streaks / loss aversion** | Duolingo, every top-grossing mobile game | Escalating daily check-in bonus (🔥), resets if you miss a day |
| **Progression & unlocks** | XP/levels everywhere | Levels unlock cash bonuses + real abalone facts; 6 tanks and 6 upgrade tracks to buy |
| **Patience pays / risk-reward choice** | Crop-tier choices in FarmVille | Harvest at Medium for quick cash, or wait for Jumbo worth 6× more |
| **Production chains** | Hay Day's raw → product multiplier | Fresh abalone sell cheap; canning them (3 → 1 can, takes time) multiplies value |
| **Market timing** | Hay Day roadside-shop pricing meta | Live price index oscillates 70%–135%+; sell when it's green |
| **Collections & achievements** | Pokémon-style completionism | 13 achievements paying pearls (premium currency), 16 collectable facts |
| **Numbers go up** | Idle-game compounding | Upgrades: auto-feeder, UV filter, backup pumps, feed lab, canning line, export marketing |

Sources consulted:
- Gamification Research: How FarmVille uses Game Mechanics (Founders Network)
- Beyond 'Wither' & Supercompensation in Games (Game Developer)
- How big games use Appointment Mechanics (TheAppGuruz)
- Hay Day Dissection (Udonis)
- How to design idle games (Machinations.io)
- Daily Rewards, Streaks, and Battle Passes in Player Retention (designthegame.com)

## Core loop

1. **Stock** a tank with hatchery spat (cheap)
2. **Feed** kelp (R30) and **clean** the seawater — both decay in real time
3. **Fix pumps** when they randomly fail (tap-tap-tap mini-interaction)
4. **Harvest** at Medium, Large or Jumbo — patience pays
5. **Can** at the cannery (timed batches) for a big value multiplier
6. **Sell** on the export market — time the price index, catch export orders
7. **Reinvest** in more tanks and upgrades → bigger numbers → repeat

## The Abagold tie-in

- 16 real facts about abalone farming, Haliotis midae, Hermanus, and Abagold
  unlock as level-up rewards — education delivered as a prize, not a lecture.
- The "More" tab and end-game fact carry a call-to-action to taste real
  Abagold abalone — the CTA links straight to the online shop
  (abagold-shop-storefront-iota.vercel.app).
- Canning, export markets, kelp feed, seawater pumps and hatchery spat mirror the
  real land-based farm's operations.

## Tech

- Single self-contained HTML file (`index.html`): vanilla JS, zero dependencies,
  works on GitHub Pages, loads instantly on mobile data.
- PWA: `manifest.webmanifest` + `sw.js` → installable to the home screen and
  playable offline, portrait-locked, standalone (no browser chrome).
- One-time install offer on first open: Android/Chrome triggers the native
  install prompt via `beforeinstallprompt`; iOS Safari (no install API) shows
  the Share → Add to Home Screen steps instead. Never shown when already
  running installed, and never repeated once seen (device-level flag).
- Abalone are drawn as a hand-crafted SVG (ear-shaped shell, respiratory pores,
  growth ridges, mother-of-pearl lip, foot fringe) with a colour palette per
  growth stage — spat are pale, jumbos dark and golden.
- They crawl slowly around the tank on individual paths — steady linear glides
  with grazing pauses, the way real abalone move (pure CSS
  animations with wall-clock phase, so they never jump on re-render) and
  visibly grow in size in real time as stage progress climbs — the tank DOM
  only rebuilds on structural changes, with meters updated in place.
- Ambient background music is generated procedurally with WebAudio (slow chord
  pads, wave-wash noise, sparse kalimba melody) — no audio files, nothing to
  license, and it starts on the first tap (browser autoplay rules). Separate
  toggles for music and sound effects.
- Local player profiles, frictionless by default: first launch drops straight
  into the game as "Farmer" — no name screen. Rename and Switch farmer live in
  Settings for shared phones; each farmer keeps an isolated save slot, streak
  and progress, and the last-played farmer resumes automatically. Pre-profile
  saves migrate silently. No server, no passwords — online
  accounts/leaderboards are an app-store-phase feature.
- Tap a tank's water to zoom into a full-screen tank view: more, bigger abalone
  drifting around, and tapping an individual abalone shows an inspection card
  (stage, shell length, weight, condition, flavour line). Feed/clean/fix/harvest
  work from inside the zoom view.
- Save game in `localStorage`, autosaved every 10s and on tab hide.
- Time-based simulation with timestamp catch-up → honest offline progress.
- Simple WebAudio synth for taps/coins/fanfares (mutable).

## Roadmap to the Apple & Google stores

1. **Now**: share the GitHub Pages URL; players can "Add to Home Screen" today.
2. **Wrap**: use [Capacitor](https://capacitorjs.com) (or Bubblewrap/TWA for
   Android) to package this exact web game as native iOS/Android apps —
   no rewrite needed. Add PNG icons (192/512/1024) and splash screens.
3. **Store polish**: push notifications ("Your abalone are ready to harvest!"),
   Game Center / Play Games achievements, analytics, app-store screenshots.
4. **Later ideas**: red-tide emergency events, neighbour farms & gifting (social),
   abalone recipes as collectables, seasonal Lunar New Year sale events,
   real product coupon codes as high-level rewards.
