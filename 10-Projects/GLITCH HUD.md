---
type: project
status: completado
prioridad: media
inicio: 2026-08-06
area: "[[Dev]]"
up: "[[Projects MOC]]"
created: 2026-08-06
updated: 2026-08-06
tags:
  - glitch-hud
  - gaming
  - rss
  - cloudflare
  - premium-stack
---

# GLITCH HUD

Gaming news landing page estilo HUD corrupto + glitch magazine. 14 archivos, ~2800 líneas.

## Datos
- **Ubicación:** `C:\Users\rtert\Documents\glitch-hud\`
- **Stack:** HTML + CSS + JS vanilla, GSAP 3.12.7 + ScrollTrigger, Lenis 1.1.18, Three.js r128, Chart.js 4.4.7
- **Diseño:** fondo `#0a0a0f`, acento `#00ff88`, fuentes VT323 / Share Tech Mono, glassmorphism, scanlines, noise overlay
- **Ref:** [[30-Resources/Premium Design Stack|Premium Design Stack]]

## Estado: Completado
- **7 secciones:** HUD overlay, hero 3D, breaking news (6 cards live), top games (5 con Chart.js), trending tabs, newsletter, footer
- **HUD elements:** Health bar (scroll %), minimap (100x150px), crosshair (24px), status text
- **Modal noticias** con focus trap + `data-lenis-prevent`
- **Card tilt 3D** + glow en hover
- **Glitch text** on scroll con ScrollTrigger
- **Three.js reactivo** al scroll (cambia color/velocidad)
- **Live RSS:** Vandal + HobbyConsolas vía Cloudflare Worker
  - Worker: `https://glitch-hud-rss.rtertulien2811.workers.dev`
  - ISO-8859-1 decoding fix para Vandal
  - Fallback: allorigins.win
  - Cache localStorage 10min, auto-refresh 5min
- **QA:** shinobi-reviewer 83/100 PASS — 8 fixes

## Agentes usados
- **designer:** HTML + CSS base (estructura, HUD, responsive, glitch CSS)
- **data-ninja:** JS (news-data, hud, three-scene, gsap-effects, charts, app, news-api)
- **shinobi-reviewer:** QA (8 fixes: contrast WCAG, ARIA tabs, pixel ratio, reduced-motion, RAF loop, SRI hashes, orphan CSS, glitch pseudo-elements)

## Server local
```powershell
python -m http.server 8080  # desde C:\Users\rtert\Documents\glitch-hud
# Abrir http://localhost:8080
```

## Archivos
- `index.html` — estructura 7 secciones + modal + ticker
- `css/style.css` — tokens, HUD, hero, cards, tabs, form, footer, modal
- `css/glitch.css` — glitch-text, image-glitch, scanline flicker
- `css/responsive.css` — breakpoints 1024/768/480/360/1440
- `js/news-data.js` — NEWS_DATA, TRENDING_DATA, GAMES_DATA
- `js/news-api.js` — fetch RSS Worker + fallback + cache
- `js/hud.js` — crosshair, health bar, minimap, status
- `js/three-scene.js` — Three.js 3D scene + scroll reactivity
- `js/gsap-effects.js` — Lenis + ScrollTrigger + reveals + typewriter
- `js/charts.js` — Chart.js horizontal bars
- `js/app.js` — tabs, newsletter, push, magnetic hover, modal
- `worker/rss-proxy.js` — Cloudflare Worker RSS proxy
- `server.ps1` — PowerShell server (unused, prefer Python)
