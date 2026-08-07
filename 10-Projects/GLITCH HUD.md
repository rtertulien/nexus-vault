---
up: "[[Projects MOC]]"
status: activo
area: desarrollo
created: 2026-08-06
updated: 2026-08-06
tags:
  - glitch-hud
  - gaming
  - rss
  - cloudflare
---

# GLITCH HUD

Landing page gaming estilo HUD con noticias, rankings y newsletter.

## Datos
- **Ubicación:** `C:\Users\rtert\Documents\glitch-hud\`
- **Stack:** HTML + CSS + JS vanilla, Three.js, GSAP, Chart.js, Lenis
- **Diseño:** fondo `#0a0a0f`, acento `#00ff88`, fuentes VT323 / Share Tech Mono

## Estado
Live RSS implementado:
- Worker proxy en Cloudflare (`worker/rss-proxy.js`) para Vandal y HobbyConsolas.
- Módulo `js/news-api.js` con fetch, fallback a allorigins.win, cache localStorage 10min, auto-refresh 5min.
- `js/news-data.js` con arrays `CURRENT_NEWS`, `LIVE_NEWS`, `NEWS_SOURCE`.
- `js/app.js` reemplaza cards estáticos por noticias live, muestra badge LIVE, botón refresh y toast.
- Ticker GSAP en `index.html` con últimos 5 titulares.
- CSS/ responsive actualizados para ticker, badge, toast y botón refresh.

## Pendiente
1. Desplegar el Worker a Cloudflare.
2. Actualizar `RSS_PROXY_URL` en `js/news-api.js`.
3. Probar en file:// y servidor local.
