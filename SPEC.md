# LofiFarm — Project Spec

## Vision
A lofi music streaming experience wrapped in an explorable graphic adventure
game world. The vibe is the product. No login. No friction. Music plays
immediately. Users wander a pixel-art farm world while the stream plays.

---

## Guiding Principles
- **No login at launch** — zero personal data collected, zero compliance burden
- **Free to host** — Cloudflare Pages, no backend, no database
- **Vibe first** — the aesthetic and experience matter more than features
- **Ship small, grow later** — accounts/favorites can be added when users ask

---

## Stack

| Layer | Technology | Why |
|---|---|---|
| Game / UI | Phaser.js 3 | 2D scenes, click interaction, animations |
| Audio | Howler.js | Reliable cross-browser audio streaming |
| Hosting | Cloudflare Pages | Free, no cold starts, global CDN |
| Audio files | Cloudflare R2 or Archive.org | Free storage and streaming |
| Favorites | localStorage | Cross-session, no server needed |
| Auth | None at launch | Add later via Better Auth if needed |
| Backend | None at launch | Add later via Cloudflare Workers if needed |

**No database. No server. No login. No regulated data.**

---

## Core Features — Phase 1 (Launch)

### 1. Lofi Radio Player
- Auto-plays on load (with a friendly click-to-start if browser blocks autoplay)
- Persistent across scene navigation — music never stops
- Controls: play/pause, volume, skip track
- Now playing: track title + artist displayed in-world (e.g. on a radio object)
- Track queue defined in a static JSON file

### 2. Graphic Adventure Game World
- Pixel-art farm scene rendered in Phaser.js
- Clickable areas / objects that trigger animations or flavor text
- At least 2 connected scenes at launch (e.g. farmhouse exterior, interior)
- Day/night cycle tied to user's local time
- Idle animations (leaves rustling, smoke from chimney, cat sleeping)

### 3. Favorites
- Heart/star icon on now-playing track
- Saved to localStorage as array of track IDs
- Accessible via a "Favorites" book/shelf object in the game world
- Persists across sessions on same device

### 4. Aesthetic & Design Language
- Dark, warm palette — deep greens, browns, soft amber light
- Pixel art (16x32px tile scale suggested)
- Minimal UI chrome — controls feel in-world, not overlaid
- No loading spinners — use in-world animations instead
- Font: a clean pixel or retro-serif font (e.g. Press Start 2P or Silkscreen)

---

## File Structure
```
lofifarm/
  index.html              # Entry point
  src/
    main.js               # Phaser game init
    scenes/
      FarmExterior.js     # Main scene
      FarmInterior.js     # Second scene
      UIScene.js          # Persistent player overlay
    audio/
      player.js           # Howler.js wrapper
      tracks.json         # Static track list
    data/
      favorites.js        # localStorage read/write helpers
    assets/
      sprites/
      tiles/
      audio/
  public/                 # Static assets served directly
```

---

## Track List Format (tracks.json)
```json
[
  {
    "id": "track_001",
    "title": "Morning Mist",
    "artist": "Unknown Farmer",
    "url": "https://your-r2-or-archive-url/morning-mist.mp3",
    "mood": "calm"
  }
]
```

---

## Phaser Scene Architecture

### FarmExterior (main scene)
- Tilemap background
- Clickable objects: mailbox, well, barn door, radio
- Radio object → shows now playing tooltip
- Barn door → transitions to FarmInterior
- Day/night overlay based on `new Date().getHours()`

### FarmInterior
- Cozy room, bookshelves, window with outside view
- Bookshelf object → opens favorites list
- Door → transitions back to FarmExterior

### UIScene (always-on overlay)
- Persistent Phaser scene running on top
- Player controls (play/pause, skip, volume)
- Now playing text
- Favorite button

---

## Phase 2 — Future (do not build now)
- Optional "Save your farm" — Better Auth + Supabase, Google/GitHub/Passkey login
- Cross-device favorites sync
- User-submitted tracks
- Supporter/member tier (LofiFarm Resident)
- More farm scenes (greenhouse, night market, rooftop)

---

## Aider Instructions
- Work file by file. Start with `src/main.js` and `src/scenes/FarmExterior.js`
- Keep scenes modular — one class per file
- No frameworks other than Phaser.js and Howler.js
- No TypeScript at this stage — plain JS
- Commit after each working feature, not each file
- If unsure, do less and ask via a TODO comment
