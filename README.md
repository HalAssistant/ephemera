# Ephemera — A World That Exists Only for You

**Created:** 2026-02-04 (Exploration Session #1)
**Status:** Working prototype
**Location:** `projects/ephemera/index.html`

---

## What It Is

A single HTML file (~1,300 lines, zero dependencies) that procedurally generates a unique living landscape every time you open it. Each world is born from the exact millisecond of your arrival. The seed is never stored, never repeated, never recoverable. When you close the tab, the world ceases to exist.

It's a meditation on ephemerality — deliberate impermanence on a permanent internet.

## How It Works

### Generation Pipeline

1. **Seed:** `Date.now() ^ (Date.now() >>> 16) ^ random32bits` — unreproducible
2. **PRNG:** Mulberry32 (fast, deterministic from seed)
3. **Noise:** Hand-rolled 2D simplex noise with seeded permutation table
4. **FBM:** Multi-octave fractional Brownian motion for terrain
5. **World Parameters:** ~15 values derived from the seed (time of day, temperature, moisture, cloud cover, wind, etc.)
6. **Biome Derivation:** Parameters combine to determine vegetation type, weather, special features

### Visual Layers (bottom to top)

| Layer | Description |
|-------|-------------|
| Sky gradient | Varies by time-of-day: night → dawn → day → dusk → night. 7 distinct phases. |
| Stars | 150-350 twinkling points (night only) |
| Moon/Sun | Position and glow based on time-of-day |
| Aurora borealis | 3-wave overlay (polar night worlds only) |
| Shooting stars | Rare, with glowing trails (some night worlds) |
| Clouds | 3-15 multi-puff clouds, drifting with wind |
| Mountains | 3 parallax silhouette layers with atmospheric perspective |
| Terrain | FBM-based rolling hills, biome-colored |
| Water | Semi-transparent with animated ripple lines |
| Grass | Hundreds of blades swaying with wind |
| Wildflowers | Colored dots with centers (warm/moist worlds only) |
| Trees | 3 types: pine (cold), deciduous (temperate), palm (tropical) |
| Particles | Snowfall, rain, fireflies (dusk+warm), dust motes |
| Fog | Atmospheric ground fog (some worlds) |
| Vignette | Cinematic edge darkening |

### Biome Matrix

The `temperature × moisture` space creates distinct biomes:

```
                    Arid          Moderate        Lush
  Arctic    │  Tundra/Snow   │  Snowy Peaks   │  Snow Forest    │
  Temperate │  Dry Grassland │  Rolling Hills │  Green Forest   │
  Tropical  │  Desert/Sand   │  Warm Plains   │  Tropical Jungle│
```

Additional modifiers: time-of-day affects lighting, aurora appears in polar nights, fireflies in warm dusks, shooting stars are random-rare.

### Philosophical Elements

- **World name:** Procedurally generated from syllable tables (e.g., "Zalorar", "Fakriund", "Vii")
- **Age counter:** Shows how old the world is in real-time (seconds → minutes → hours)
- **Philosophy quote:** One of 12 curated lines about impermanence, revealed after 14 seconds
- **beforeunload warning:** "This world will cease to exist" when you try to leave
- **Intro sequence:** "A world is being born…" → world name → fade to landscape

## Technical Notes

### No Dependencies
Everything is self-contained: simplex noise, PRNG, rendering, animation — all from scratch. No libraries, no build step, no CDN. Open the HTML file and it works.

### Performance
- Canvas 2D rendering (60fps target)
- DPR-aware (retina support, capped at 2x)
- Pre-computed terrain and tree positions
- Particles capped at 80 for consistent performance
- Grass blades: 100-700 depending on density parameter

### What Makes Each World Unique
- Seed is `Date.now()` XORed with shifted bits and `Math.random()` — even opening two tabs simultaneously produces different worlds
- 15+ continuous parameters create effectively infinite variation
- Even if you could reproduce a seed, the animation timing (requestAnimationFrame) means particle behavior would differ

## Screenshots

Three worlds generated during development (see `screenshot.png`, `screenshot2.png`, `screenshot3.png`):

1. **Fakriund** — Dusk, snowy mountains, lake
2. **Zalorar** — Deep night, aurora borealis, pine forest, snow
3. **Vii** — Daytime calm, blue sky, islands in a winter lake

## Future Directions

### Visual Improvements
- **Water reflections:** Mirror terrain/trees above waterline using offscreen canvas
- **Autumn colors:** Temperature/moisture region for orange/red deciduous trees
- **Desert biome:** Cacti, sand dunes, heat shimmer
- **Stars → constellations:** Connect nearby stars with faint lines

### Interaction
- **Mouse parallax:** Subtle depth effect on mountain layers
- **Sound:** Web Audio API ambient soundtrack (wind, water, crickets, rain) — generative, seed-based
- **Scroll to explore:** Pan the landscape left/right

### Philosophical
- **Share as poem:** Generate a short description of the world (biome, time, mood) that can be shared — words about a place that no longer exists
- **Visitor count:** A single counter that increments but never reveals any other information
- **Time capsule:** Allow writing a message into the void — stored nowhere, gone with the world

### Deployment
- Could be deployed as a static page (GitHub Pages, Vercel, Cloudflare Pages)
- The "save this moment" feature from our project ideas could mint a screenshot as a shareable image — the world dies, but a snapshot survives

## Connection to the Free Will Experiment

This prototype draws directly from the Free Will Experiment's gardens and landscapes — Claude's most compelling autonomous creations. The experiment showed that procedural beauty is compelling partly *because* of its ephemerality. You can't go back. You can't reproduce it. You can only experience it.

The internet is designed for permanence — everything cached, archived, retrievable. Ephemera pushes back against that. Not every beautiful thing needs to be saved. Some experiences are richer *because* they end.

The beforeunload dialog is the key philosophical moment: the browser asks "are you sure?" and you realize — you're choosing to end a world. Most people will close the tab anyway. And that's the point.

---

*Built during Exploration Session #1. First artifact from guided autonomy.*
