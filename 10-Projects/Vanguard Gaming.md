---
up: [[Nexus]]
status: activo
type: proyecto
stack: TypeScript, GSAP, Three.js, GNews API, MyMemory API, Cloudflare Pages
created: 2026-08-14
---

# Vanguard Gaming

## Resumen

Gaming news portal landing page. Top 10 noticias con API real (GNews), traducción EN→ES (MyMemory), loading glitch, hero Three.js wireframe, GSAP animations, morphing modal, dark professional theme.

## Stack

- TypeScript
- GSAP 3.15 (ScrollTrigger)
- Three.js r128
- Fontshare (Clash Display + Satoshi)
- GNews API (100 req/día)
- MyMemory API (5000 chars/día)
- Cloudflare Pages + Functions
- Cloudflare Worker proxy

## APIs

- **GNews:** `GET /api/gnews/top-headlines` via proxy server-side (`functions/api/gnews/[[path]].ts`), API key in CF env secrets.
- **MyMemory:** direct client-side `GET https://api.mymemory.translated.net/get?q=...&langpair=en|es`, 500 chars max/query.

## Arquitectura

Single Page Landing (Loading → Header → Hero → Top 10 → Detail Modal → Footer). Vanilla JS frontend (app.js ~478 lines), TypeScript API layer (src/api/ not yet integrated to frontend), proxy CF Function. Build: `scripts/build.js` → `dist/`.

## Agentes

9 fases con outcome loop. investigador (APIs), data-ninja (integración TS), designer (×3: HTML/CSS, loading/GSAP, Three.js/grid/modal), shinobi-reviewer (QA), obsidian-curator (docs). Fases 1-8 completadas. QA FAIL 62→71→fixes manuales.

## Estado

MVP funcional con mock data + fallback real API. Pendiente: integrar capa TS al frontend, deploy real en CF Pages con API key configurada.

## Archivos

- index.html
- css/style.css (~1378 lines)
- js/app.js (~478 lines)
- js/three-scene.js
- src/api/{types,gnews,translate}.ts
- src/utils/{cache,format}.ts
- functions/api/gnews/[[path]].ts
- scripts/build.js
- assets/img/placeholder-{1-10}.svg

## Ubicación

`C:\Users\rtert\Documents\vanguard-gaming\`

## Próximos pasos

1. Integrar TS al frontend vanilla
2. Deploy CF Pages con GNEWS_API_KEY real
3. Probar en producción
