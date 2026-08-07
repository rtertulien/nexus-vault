---
status: activo
created: 2026-07-26
updated: 2026-07-27
tags: [spotify, wrapped, cosmic-neon, aurora-nova, vanilla-js, pwa, oauth]
up: "[[Nexus]]"
---

# Spotify Wrapped Cosmic Neon — Aurora Nova

Web app year-in-review estilo Spotify Wrapped. Rediseño visual "Aurora Nova" — cada slide es un evento cósmico único.

**Ubicación:** `C:\Users\rtert\Documents\spotify-wrapped\`

**Stack:** HTML + CSS + vanilla JS, Three.js r128, GSAP 3.12.5, PKCE OAuth Spotify.

**Estado:** v24 — Aurora Nova implementado, bugs críticos corregidos, funcional.

---

## Aurora Nova v24 — Resumen del rediseño

### Paleta refinada
- Fondos: void `#000`, deep `#050510`, elevated `#0a0a1a`
- Neons: purple, cyan, pink, amber, green, **aurora `#00ffa3`**, **plasma `#ff2d92`**, red, coral
- Pearlescent whites: `#f8fafc`, `#e2e8f0`, `#cbd5e1`
- 11 aurora conic-gradient únicos por slide

### Tipografía triple
- **Boldonse** (display) → hero mega, números stats
- **Fraunces** italic (editorial) → títulos de sección, arquetipo DNA
- **Outfit** (body) → todo lo demás
- **JetBrains Mono** → IDs, datos numéricos

### 11 slides con identidad visual
1. Hero → Supernova (mega "WRAPPED" liquid metal, CRT scanlines, aurora gradient)
2. Artists → Constellation (aurora border cards, rank watermark 6rem, glow ring images)
3. Tracks → Orbit (album art atmosphere, waveform bars, rank watermark 5.5rem)
4. DNA → Nebula (goo filter SVG, aura pulsante, Fraunces italic archetype)
5. Genres → Galaxy Cluster (nebula bubbles, nebulaFloat animation)
6. Patterns → Pulsar (peak ring pulse heatmap, typing insights)
7. Stats → Cosmic Rays (bento grid, spring counters, shimmer rainbow)
8. Details → Stellar Wind (wind trail hover effect)
9. Gem → Meteor (bounce entrance, aurora border, gemFloat)
10. Evolution → Star Birth/Death (comparison cards, arrow pulse)
11. Share → Comet (3D tilt preview, comet trail, launch button)

### Three.js + GSAP enhancements
- 9 particle modes reactivos por slide (burst/orbit/flow/pulse/rays/wind/meteor/birth/comet)
- Camera Z-depth transitions por slide (250-350 range)
- Burst particle system (200 particles en hero)
- Mouse magnetic effect (120px radius)
- GSAP cinematic: parallax 3-layer, entrance scale 0.92→1 blur 12→0, nav dot bounce

---

## Arquitectura (14 archivos, ~9000+ líneas)

| Capa | Archivos | Propósito |
|------|----------|-----------|
| Auth | `js/auth.js` | PKCE OAuth, refresh token, CSRF state, console.error en 5 puntos |
| API | `js/api.js` | Spotify + Last.fm APIs, rate limit, AbortController 15s |
| Data | `js/data.js` | Procesa datos, genres Last.fm, hidden gem sin popularity |
| Charts | `js/charts.js` | Radar SVG, constelación géneros, heatmap |
| Slides | `js/slides.js` | 11 slides con GSAP cinematic, parallax, scrambleText, magnetic hover |
| Particles | `js/particles.js` | Three.js 1000 particles, 9 modes, burst, camera transitions |
| Sound | `js/sound.js` | Web Audio API synth pads (no Howler), 11 chord progressions |
| Share | `js/share.js` | Canvas 1080×1920 PNG + DOM preview |
| App | `js/app.js` | Orquestador: init→auth→load→render. style.display show/hide |
| CSS | `variables.css`, `base.css`, `animations.css`, `components.css`, `slides.css` | Aurora Nova tokens, noise grain, 21+ keyframes, 2200+ líneas |
| HTML | `index.html` | Google Fonts (Boldonse/Fraunces/Outfit/JetBrains), SVG portal logo |

---

## Bugs corregidos (v18→v24)

1. `#slides-container` → `#slides-wrapper` (JS reference mismatch)
2. particles.js halfW/halfH undefined (declared at module level)
3. sound.js removed Howl guard (uses Web Audio, not Howler)
4. Share preview index 9→10
5. 6 missing CSS vars added (--glow-green, --glow-amber-strong, etc.)
6. Loading/error/retry IDs added to HTML
7. **App.init() never called** — added at end of app.js
8. **CRITICAL OAuth loop** — migrated from hidden attribute to style.display
9. auth.js console.error at 5 OAuth failure points
10. Duplicate nav-dots/slide-counter removed from components.css
11. Hero banner margin-bottom 60px (separar del scroll hint)
12. Stats grid gap 12→16px, icon 32→36px

---

## API Spotify (julio 2026)

- PKCE OAuth — **requiere servir desde `http://127.0.0.1:8080`**
- `audio-features` **deprecated** — fallback a simulated data
- `popularity` **eliminado** — hidden gem sin dependencia de popularity
- `genres` puede venir **vacío** de la API

---

## Pendientes del usuario

1. Crear app en developer.spotify.com → CLIENT_ID real en `js/auth.js`
2. Configurar redirect URI en dashboard
3. Añadir email al allowlist (máx 5 en modo dev)
4. Servir con: `python -m http.server 8080 --bind 127.0.0.1`

---

## Agentes usados

1. **investigador** — API Spotify status, PKCE, deprecations
2. **content-curator** — UI/UX Wrapped research
3. **supervisor** — Arquitectura, features
4. **data-ninja** — Data pipeline, rate limits
5. **designer** — Aurora Nova proot (2825 líneas), CSS premium
6. **shinobi-reviewer** — QA completo (12+ fixes en sesión 27 jul)
7. **obsidian-curator** — Documentación vault

## Memoria

Guardado en memoria vectorial con tags: `spotify wrapped aurora-nova cosmic-neon v24`
