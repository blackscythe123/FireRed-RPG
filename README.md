# FIRERED — a fan-made, GBA-style browser RPG

> **About this project:** this is a trial test of the new Claude model **"Fable 5"**.
> The entire game — engine, battle system, data, maps, UI, audio, and mobile
> support — was generated from a **single-shot prompt** (reproduced in full at
> the bottom of this README), then iterated with bug-fix passes.
>
> Non-commercial fan project for AI-capability testing. **Pokémon is
> © Nintendo / Creatures Inc. / GAME FREAK Inc.** This project is not
> affiliated with, endorsed by, or connected to them in any way.

A single-page monster-catching RPG in plain HTML/CSS/JavaScript. No build
tools, no dependencies, no server — open `index.html` and play, or deploy the
folder as a static site.

## Run

- **Local:** open `index.html` directly (file:// works — plain script tags, no modules).
- **Deployed:** any static host serves it as-is (see Deploy below).

## Controls (desktop)

| Key | Action |
| --- | --- |
| Arrows / WASD | Move / navigate menus |
| Z / Space | A — confirm, talk, interact |
| X / Esc / Backspace | B — cancel, back |
| Enter | Confirm; opens the menu in the overworld |
| M | Toggle sound |

## Mobile controls & PWA

The game is fully playable on phones, in **landscape only**:

- Portrait shows a full-screen "Please rotate your device" overlay
  (plus a best-effort `screen.orientation.lock('landscape')` attempt).
- **True fullscreen on every device:** the logical viewport *widens*
  (240–360 logical px) to match the screen's aspect ratio, then scales —
  no letterboxing on desktop or phones. Press **F** for browser
  fullscreen on desktop. The page itself never scrolls or bounces.
- Touch controls are overlaid **inside the game** like mobile RPG ports:
  semi-transparent **D-pad** bottom-left (hold to keep walking), **A / B**
  bottom-right with the **START** pill stacked above A — all driving the
  same input layer as the keyboard.
- Controls appear only when touch is the **primary** input (coarse pointer
  + no hover) — touchscreen laptops stay in keyboard mode; a real tap shows
  the controls, a keypress hides them again.
- **Installable PWA**: `manifest.json` (fullscreen display, landscape
  orientation, pixel-Charizard app icon on a fire-red background) plus a
  cache-first service worker, so the deployed game can be added to the
  home screen and played offline.

## Progression

1. Wake up in your room, head downstairs — MOM sends you off with a Potion.
2. Choose Bulbasaur, Charmander, or Squirtle at Prof. MAPLE's lab.
3. Your rival REX grabs the type-advantaged starter and battles you on the spot.
4. FERNWAY TRAIL: tall-grass wild encounters (Pidgey, Rattata, Caterpie,
   Weedle, Lv 2–5) and two line-of-sight trainers.
5. STONEGATE: a classic red-roof **Pokémon Center** (free healing), a
   blue-roof **Poké Mart** (shop), and the gym — beat Trainee ROCCO and
   Leader MASON for the GRANITE BADGE.
6. Save anytime — browser slot or a portable save code you can import from
   the title screen.

## Mechanics (Gen-3-modeled)

- Gen 3 damage formula with STAB, 85–100% variance, criticals (1/16; 1/8
  high-crit), and the full real type chart (all 17 types).
- Stats HP/Atk/Def/SpA/SpD/Spe; speed order; ±6 stat stages **including
  accuracy and evasion**.
- Status: poison, burn (halves physical Atk), paralysis (¼ speed, 25% full
  para), sleep (2–4 turns), freeze (20% thaw; fire moves thaw). Fire-types
  can't be burned; Poison/Steel-types can't be poisoned.
- Volatiles: flinch, confusion (2–5 turns, 50% self-hit), Leech Seed.
- Multi-hit moves (Fury Attack, Pin Missile, Twineedle), Super Fang,
  priority (Quick Attack), PP and Struggle.
- Gen 3 catch formula (HP, catch rate, ball bonus, status bonus) with a
  focused capture sequence: HUD clears, suck-in shrink, up to three wobbles
  with beeps, lock click + star burst — or a break-open flash with flying
  shell fragments on escape.
- **Later-gen EXP Share:** every able party member earns experience from
  each KO — participants get the full amount, benchwarmers half.
- **Shiny encounters (1/512):** every generated creature can roll shiny,
  with real shiny sprites, a sparkle + callout on wild entry, and shiny
  thumbnails in the party screen.
- Trainer battles show the opponent's team as ball icons that darken as
  each one faints; trainers watch **all four directions** — you can't
  sneak behind them.
- Battle presentation: real battleback art (cover-scaled to the viewport),
  soft ground shadows under both combatants, per-move-type canvas hit
  effects (flames, droplets, jolts, leaves, powder, slashes, rings) and
  screen shake.
- Level-ups, move learning with replace prompt, level evolutions
  (starters 16, Caterpie/Weedle lines 7→10, Pidgey 18, Rattata 20, mid
  evolutions 32/36). Blacking out returns you to the most recently visited
  healing center (home before you've reached one).

## Assets

- All images are **downloaded locally** into `assets/` by two dev scripts
  (`node download-assets.js`, `node download-overworld.js`) — the game never
  hotlinks at runtime and works fully offline.
- In-game loading is **lazy with a background warm-up**: nothing blocks the
  page; every image is created on demand, and a `preloadAll()` pass right
  after first paint caches the rest (including all shiny variants) while
  you're still on the title screen.
- Battle sprites (front/back) and item icons:
  [42arch/pokemon-dataset-zh](https://github.com/42arch/pokemon-dataset-zh)
  (dataset/images reference) with [PokeAPI/sprites](https://github.com/PokeAPI/sprites)
  as the actual source for most front sprites, all back sprites, and item
  icons (the dataset's image paths weren't directly fetchable).
- Overworld art — tilesets, building exteriors, character walk-cycle sheets,
  and the battle background — comes from
  [torresflo/Pokemon-Obsidian](https://github.com/torresflo/Pokemon-Obsidian)
  (a PSDK/Pokémon Studio fan game). GPL-3 applies to that project's *code*;
  the artwork itself remains Nintendo-derived.
- The renderer slices 32px tileset cells onto the game's 16px grid, draws
  whole-building images over grassed footprints, and animates 4-direction
  × 4-frame walk cycles anchored at the feet. If any image is missing or
  fails to load, every surface **falls back to the original code-drawn
  art**, which stays in the codebase.
- Sprite/character/tile designs **© Nintendo / Creatures Inc. / GAME FREAK
  Inc.** — used here non-commercially for a model-capability test; this
  project is not affiliated with or endorsed by them.

## Deploy (Vercel)

1. Push this folder to a GitHub repo (it's already a git repo — just add a
   remote and push).
2. In Vercel: **Add New → Project → Import** the repo.
3. Done. `vercel.json` pins it as a zero-build static site (`framework: null`,
   no build command, `index.html` at the root, long-lived cache headers for
   `assets/`, day-long for `js/`/`css/`).

## Code layout

| File | Role |
| --- | --- |
| `js/main.js` | Game state, title screen, main loop, mobile setup |
| `js/overworld.js` | Tile engine, movement, NPCs, LOS trainers, warps, menus |
| `js/battle.js` | Battle system, catch logic, hit effects, screen shake |
| `js/ui.js` | Input (keyboard + touch), typewriter dialog, menus, HUD |
| `js/save.js` | Browser save slot + portable save codes |
| `js/audio.js` | Web Audio SFX (synthesized, including the title roar) |
| `js/data/*.js` | Types, moves, species, items, maps, drawn sprites, image assets |
| `AGENTS.md` / `CLAUDE.md` | Continuation guides for AI agents (see below) |
| `assets/sprites/`, `assets/items/` | Battle sprites (front/back + shiny variants) + item icons |
| `assets/overworld/` | Tilesets, building images, flower autotile |
| `assets/characters/` | 4-direction walk-cycle sheets (player + NPCs) |
| `assets/battle/` | Battle background |
| `assets/icons/`, `assets/og-image.png` | PWA app icons + link-preview card |
| `manifest.json` / `sw.js` | PWA manifest (fullscreen, landscape) + offline service worker |
| `download-assets.js` | Dev script: battle sprites + item icons |
| `download-overworld.js` | Dev script: Pokemon-Obsidian overworld art |
| `validate.js` | Dev-only data integrity checker (`node validate.js`) |
| `vercel.json` | Zero-config static deployment |

## For AI agents continuing this project

Two guides ship with the repo so any capable model can pick the work up at
the same quality bar:

- **`AGENTS.md`** — the canonical guide: architecture map, the hard rules
  (no build step, fallback-first rendering, local-only assets, validation
  gates, single-amended-commit git workflow, originality posture), the
  sharp edges already hit (one-shot input semantics, decal body geometry,
  pixel-snapping rules), and the quality bar for new work.
- **`CLAUDE.md`** — Claude-specific workflow notes on top of AGENTS.md
  (validation commands, tooling caveats, where to start per task type).

## Debug helpers (browser console)

```js
DEBUG.give('pidgey', 12)   // add a team member
DEBUG.heal()               // restore the party
DEBUG.money(99999)
DEBUG.warp('stonegate', 9, 13)
```

---

## The single-shot prompt

The prompt below is self-contained and would recreate this system from
scratch in one shot.

```text
Build a complete, playable, FireRed-inspired monster-catching RPG as a pure
static web app in a folder called pokemon-rpg. Requirements:

TECH
- Vanilla HTML + CSS + JavaScript only. No build tools, no modules, no
  server: it must run by double-clicking index.html (file://) and deploy
  unchanged as a static site (include a vercel.json with framework null and
  no build step, plus cache headers for js/css/assets).
- Organize the code into separate files: engine/overworld, battle system,
  UI/input, save system, audio, and a js/data/ folder with types, moves,
  species, items, maps, drawn sprites, and image-asset loading.

DATA (real, Gen 3 values)
- The first 20 National Dex Pokémon (Bulbasaur through Raticate) with their
  real names, types, Gen 3 base stats, catch rates, base EXP yields,
  FireRed level-up learnsets, and evolution levels (starters 16,
  Caterpie/Weedle 7 then 10, Pidgey 18, Rattata 20, mid evolutions 32/36).
- Real moves with actual power/accuracy/PP/type and the Gen 3 by-type
  physical/special split: Tackle, Scratch, Growl, Tail Whip, String Shot,
  Sand Attack, Smokescreen, Sweet Scent, Scary Face, FeatherDance, Growth,
  Harden, Withdraw, Agility, Supersonic, Vine Whip, Razor Leaf, Leech Seed,
  Sleep Powder, Stun Spore, PoisonPowder, Poison Sting, Ember, Flamethrower,
  Metal Claw, Slash, Bubble, Water Gun, Bite, Rapid Spin, Quick Attack,
  Gust, Wing Attack, Twister, Fury Attack, Pin Missile, Twineedle,
  Confusion, Psybeam, Hyper Fang, Pursuit, Super Fang, Struggle.
- The complete real 17-type Gen 3 type chart.
- Real items at FireRed prices: Potion 300, Super Potion 700, Antidote 100,
  Paralyze Heal 200, Awakening 250, Burn Heal 250, Ice Heal 250,
  Full Heal 600, Poke Ball 200, Great Ball 600.

BATTLE SYSTEM (Gen 3 mechanics)
- Fight / Bag / Switch / Run menu; up to 4 moves with PP; Struggle when dry.
- Gen 3 damage formula: ((2L/5+2) * power * A/D)/50 + 2, then crit x2
  (1/16 base, 1/8 high-crit), STAB 1.5, type effectiveness, random 85-100%.
- Stat stages -6..+6 for Atk/Def/SpA/SpD/Spe AND accuracy/evasion
  (3-based multiplier), burn halving physical attack, paralysis quartering
  speed with 25% full paralysis, sleep 2-4 turns, freeze with 20% thaw and
  fire-move thawing, poison/burn chip at 1/8 max HP.
- Volatile conditions: flinch (Bite/Hyper Fang/Twister), confusion
  (2-5 turns, 50% chance to hit yourself with a 40-power typeless physical),
  Leech Seed (1/8 drain healing the opponent, fails on Grass types).
- Multi-hit distribution 2/2/3/3/4/5 for Fury Attack and Pin Missile;
  Twineedle hits twice with 20% poison per hit; Super Fang halves HP.
- Status immunities: Fire can't be burned, Poison/Steel can't be poisoned.
- Catching: Gen 3 catch formula a = (3M-2H)*rate*ball/(3M) * status bonus
  (2x sleep/freeze, 1.5x others), b = 1048560/sqrt(sqrt(16711680/a)), four
  16-bit shake checks. Stage the capture for focus: hide the HUD boxes,
  throw arc, suck-in shrink, up to three visible wobbles each with a beep,
  then a lock click with a star burst on success — or a break-open white
  flash with red/white shell fragments plus screen shake on escape. Caught
  Pokémon join the party (max 6) or a vault.
- Later-gen EXP Share: EVERY able party member earns EXP per KO — base
  yield*level/7 (x1.5 vs trainers) in full for battle participants, half
  for the bench. Cubic level curve, stat growth on level-up, learnset
  prompts with "forget a move?" replacement, evolution after battle.
- Shiny system: every generated creature rolls shiny at 1/512; download the
  real shiny front/back sprites too, sparkle + callout when a shiny wild
  appears, shiny thumbnails in the party screen.
- Trainer battles show the opponent's team as ball icons on the enemy HUD
  box, darkening one per faint. Trainers in the overworld watch all four
  directions — approaching from behind still triggers the battle.
- Turn order by priority then effective speed; simple AI that prefers
  super-effective damaging moves 60% of the time.
- Canvas-rendered battle scene: enemy front sprite and player back sprite,
  HP boxes with animated color-shifting bars (numbers on BOTH boxes), EXP
  bar, per-move-type particle hit effects (flames, droplets, electric
  streaks, leaves, poison bubbles, psychic rings, rock chunks, slash
  streaks) and screen shake on impact.

WORLD & PROGRESSION (FireRed opening arc, original writing)
- Beat-for-beat: wake up in your upstairs bedroom -> downstairs, a parent
  NPC sends you off with a Potion (and offers free healing later) -> the
  professor's lab in town -> choose 1 of 3 starters from desks -> a cocky
  rival immediately picks the type-countering starter and battles you in
  the lab, then leaves.
- Three connected outdoor areas: starting town (player house, rival house,
  lab), a north route with tall-grass random encounters (Pidgey/Rattata/
  Caterpie/Weedle Lv2-5, ~14% per step) and two line-of-sight trainers who
  spot you, walk up, and battle; and a second town with a classic red-roof
  Pokemon Center (nurse heal flow, healing machine, benches), a blue-roof
  Poke Mart (clerk, shelves, buy flow with quantity picker), and a
  Rock-themed gym: a trainee with Harden-walls plus Leader MASON
  (Pidgeotto 11 / Raticate 13) who awards the GRANITE BADGE.
- Tile-based overworld: grid movement (arrows/WASD) with tap-to-turn,
  collision, camera, map links and door/stair warps with fades, signs,
  multi-page typewriter NPC dialogue for every NPC (parent, professor,
  rival, hint-giving townsfolk, nurse, clerk, trainers with pre/post lines),
  a start menu (Party with reordering, Bag with item use, Badges, Save),
  blackout-to-respawn on defeat with halved money.
- All character names, town names, and dialogue must be original writing -
  do not copy any text from the actual games.

ASSETS
- Write a node script that downloads, into a local assets/ folder, front
  battle sprites, back sprites, SHINY front/back variants, and item icons
  for the 20 species and 10 items from
  github.com/42arch/pokemon-dataset-zh, falling back to
  github.com/PokeAPI/sprites raw URLs for anything missing; verify PNG
  magic bytes. The game must reference only the local files (offline-safe).
- Write a second node script that downloads real overworld art from
  github.com/torresflo/Pokemon-Obsidian (inspect the repo's
  Obsidian/Graphics tree via the GitHub API first): an outdoor tileset and
  an interior tileset, whole-building images (house, healing center, mart),
  a flower autotile, 4-direction x 4-frame RMXP-style character walk sheets
  for the hero and every NPC kind, and a grass battle background.
- Renderer requirements for that art: run the canvas at 2x internal
  resolution with a setTransform(2,0,0,2,0,0) so 32px sources land on
  device pixels 1:1; slice 32px tileset cells onto the 16px logical grid
  via per-legend-char source-rect maps (separate indoor / outdoor tables,
  multi-layer tiles with a base tile under transparent or taller-than-tile
  art such as trees and shelves); snap the camera to whole device pixels
  to avoid scroll seams; draw whole-building images over grass-rendered
  footprints — MEASURE each building's opaque pixel bounds first (the
  files carry transparent shadow padding) and anchor/collide using that
  body rect, bottom-centered on the door row, marking body-covered tiles
  solid except the door; animate character walk cycles from the step tween
  with feet-anchored frames at native scale and an idle frame when
  standing; draw the battle background cover-scaled with soft ground
  shadows under each combatant instead of flat platform ellipses.
- Asset loading must be lazy (Images created on first use) plus a
  background preloadAll() shortly after first paint so page load is
  unaffected but everything is cached early.
- GRACEFUL FALLBACK everywhere: also implement simple code-drawn pixel
  tiles/characters/creature sprites, and use them automatically for any
  image that is missing or fails to load.
- Blackout respawn: blacking out returns the player to the most recently
  ENTERED healing center (set on entry, not on heal); home until then.
- Also ship AGENTS.md and CLAUDE.md continuation guides covering the
  architecture, the no-build/fallback/local-asset rules, the validation
  gates, and the git amend-into-one-commit workflow.
- Title screen: FIRERED logo text, warm fire-gradient background with
  drifting embers, a large Charizard front sprite, a synthesized roar on
  the first key press, and the menu placed so the sprite stays visible.

UI / POLISH
- GBA-style bordered dialog boxes with typewriter text (skip on press),
  bottom-right 2x2 battle menu, move panel with PP/type, party screen with
  HP bars and status chips, bag and shop screens with real item icons,
  battle intro flash/fade transition, retro pixel font (Press Start 2P with
  monospace fallback), synthesized Web Audio SFX for every interaction
  (cursor, confirm, hits by effectiveness, faint, heal jingle, level-up,
  catch sequence, badge fanfare), M to mute.
- Save system: localStorage slot plus an exportable/importable base64 save
  code (title screen has CONTINUE / NEW GAME / IMPORT CODE).

MOBILE / FULLSCREEN
- Proper viewport meta (width=device-width, initial-scale=1,
  user-scalable=no), fullscreen-friendly meta tags. The page itself never
  scrolls or bounces (overflow hidden, position fixed in touch mode,
  touchmove preventDefault outside textareas).
- Landscape-only on touch devices: CSS portrait overlay saying "Please
  rotate your device" plus a try/catch screen.orientation.lock attempt.
- TRUE fullscreen everywhere: the logical viewport WIDENS (240-360 logical
  px) to match the screen aspect, the canvas and stage resize with it, and
  the stage scales via transform — no letterboxing on desktop or phone.
  F toggles browser fullscreen on desktop. The battle scene stays a 240px
  composition centered in the wider viewport with the backdrop
  cover-scaled across it.
- On-screen touch controls overlaid INSIDE the game like mobile RPG ports:
  semi-transparent D-pad bottom-left (press-and-hold keeps walking), A and
  B bottom-right with the START pill stacked above A, >=48px targets,
  touchstart/touchend with preventDefault, wired into the exact same input
  layer as the keyboard.
- Controls show only when touch is the PRIMARY input (pointer: coarse AND
  hover: none) — touchscreen laptops stay keyboard-first; a real tap shows
  the controls and a keypress hides them.

QUALITY
- Include a node-runnable validate.js that checks map row widths, warp
  targets landing on walkable tiles, learnset/species/item references, and
  sprite data integrity. All JS must pass node --check.
```
