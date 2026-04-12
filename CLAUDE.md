# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Hamster is a virtual pet web app — a 3D hamster that lives in the browser, demands food and care, generates speech via Markov chains, and quotes religious scripture. It is a single self-contained HTML/JS file with no build step. Run by opening the file in a browser. Persistence via `localStorage`.

**Spec:** See `hamster-initial-spec.md` for the full feature list.

## Running

No build step. Open `index.html` directly in a browser. No server required.

## Architecture

### Single-file constraint
Everything lives in one `.html` file. External libraries (e.g. Three.js) may be loaded from CDN via `<script>` tags. No runtime external API calls.

### Core subsystems

| Subsystem | Responsibility |
|---|---|
| **3D Renderer** | Three.js scene: hamster mesh, animations, room environment |
| **Behavior Engine** | State machine driving hamster actions (idle, eating, sleeping, worried, playing dead, wandering, looking at user) |
| **Speech System** | Markov chain text generator → browser Web Speech API (`speechSynthesis`). Religious quotes are always verbatim. Hamster is always formal and never swears. |
| **Interaction Layer** | Mouse events: double-click to summon, click poop pellets, drop food, click sleeping hamster |
| **Persistence** | `localStorage` wrapper: user name, first-run timestamp, interaction history |
| **Time Tracker** | Calculates elapsed time since first run for milestone speech events |

### Behavior state machine (key states)
- `idle` — default, eye-tracking mouse sometimes
- `hungry` — demands food; food drop triggers eating animation
- `lotion` — requests lotion ~2x/day; only giveable when asked
- `substance` — demands "the substance"; sleeps for extended period after
- `looking` — stares at user, triggered by double-click or randomly
- `worried` — "presence" detected; fast/jerky movement, growling sounds, looks around room
- `wandering` — exits frame, returns
- `dead` — lies rigid on back; click wobbles it
- `pooping` — deposits pellets; pellets grow legs if left >1 hour; click to clean

### Speech generation
- Markov chain trained on inline corpus of hamster-appropriate phrases
- All utterances prefix the user's name: *"Peter, I have been reading the news."*
- Religious quotes (KJV Bible, Apocrypha, Gnostic Gospels) are embedded verbatim (~200 KB of verses) and spoken as-is
- One-time milestone messages keyed to elapsed time (e.g. "I have been with you for 14 days, Peter")

### Persistence schema (`localStorage`)
- `hamster_name` — user's name (prompted on first run)
- `hamster_first_run` — ISO timestamp of first run
- `hamster_milestones` — JSON array of triggered milestone IDs
- `hamster_food` — selected food type
- `hamster_total_uptime` — cumulative seconds the app has been open
- `hamster_floor_food` — JSON array of `{x, z}` floor food positions
- `hamster_stats` — JSON object: feedCount, lotionCount, substanceCount, poopPickedUp
- `hamster_poop` — JSON array of `{x, z, spawnedAt}` poop pellet positions
- `hamster_damage` — JSON array of `{x, z, rz, born}` floor scratch mark positions
- `hamster_pos` — JSON `{x, z, ry}` hamster position and facing direction

**All new persistent state must be added here and wired up with `_save()` / `loadSaved()` calls. After every code change, check: does any new state need saving? Does the `clear` command (dev console + settings menu) include the new key?**

## Implementation Phases

### Phase 1 — Scaffold & 3D hamster
- Single HTML file with Three.js from CDN
- Basic 3D hamster mesh (procedural geometry or simple GLTF)
- Scene setup: lighting, camera, simple room/surface
- Idle animation loop

### Phase 2 — Behavior engine & interactions
- State machine with `idle`, `hungry`, `eating`, `looking`, `wandering`
- Double-click to trigger "looking at you" state
- Food UI element: pick up and drop on hamster
- Mouse eye-tracking (probabilistic)

### Phase 3 — Speech system
- Markov chain generator (trained on inline corpus)
- Web Speech API integration
- Name prompt on first run; name prefixed to all speech
- Embed ~200 KB of religious verses (KJV, Apocrypha, Gnostic Gospels)

### Phase 4 — Advanced behaviors
- `substance`, `lotion`, `worried`, `dead` states
- Poop pellets with leg-growing timer
- Bug encounters (enter area, studied, possibly eaten)
- Gun/trap/police contact remarks
- Milestone speech events (time-since-first-run)

### Phase 5 — Polish
- Sound effects (lotion noise, growling, etc.) via Web Audio API
- Refined animations per state
- Menu UI: name change, food type selection
- Edge case handling, `localStorage` migration

## Future: News Integration
Per spec note: may eventually pull headlines from selected sites (e.g. BBC News, BBC Weather) and generate comments like *"Peter, I am worried about the Iran situation."* This would require a CORS-friendly proxy or user-granted fetch permission. Keep speech generation generic enough to accept external topic injection.
