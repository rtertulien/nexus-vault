---
type: project
status: en-pausa
prioridad: baja
inicio: 2026-06-14
area: "[[Dev]]"
up: "[[Dev]]"
related: ["[[Projects MOC]]", "[[Dev]]"]
tags: [project, makimono, manga, pwa]
stack: [kitsu, kavita, tailscale, kover, cloudflare-pages]
---

# Makimono

**Estado:** Kavita + Tailscale OK, Makimono MangaDex abandonado, v2 tracking pendiente
**Stack:** Kavita + Tailscale + Kover (lectura), Kitsu API (futuro)

## Fases
### Fase 1 — MangaDex (abandonado 14 Jun)
- PWA lector manga con MangaDex API
- Selector grupos scanlation, reader con tap/navegación
- Abandonado por API MangaDex inestable

### Fase 2 — Kavita + Kover (completado 15 Jun)
- Kavita nativo Windows en `localhost:5000`
- Librería: `C:\Manga\` (75 mangas)
- Tailscale VPN WireGuard: laptop `100.64.119.90`, iPhone `100.83.224.44`
- Kover (TestFlight beta): descarga offline funcional

### Fase 3 — Makimono v2 (diseño listo, pendiente julio)
- App seguimiento mangas + descubrimiento con Kitsu API
- 4 tabs: Mi Lista | Tendencias | Buscar | Mix
- Diseño oscuro premium: azul-negro `#0a0a1a`, acento azul hielo `#60a5fa`, cards blancas `#f1f5f9`
- Glassmorphism, spring animations, ripple effect, staggered grids
- Login Kitsu via proxy CF
- Architecture: Shinobi Track clone (probado), JS vanilla
- Coste estimado: 3-4h implementación

## Enlaces
- Código frontend: `C:\Users\rtert\Documents\makimono\`
- Proot: `C:\Users\rtert\Documents\proot\proot-makimono.md`
- Setup Kavita proot: `C:\Users\rtert\Documents\proot\makimono-kavita-setup.md`

## Pendientes
- [ ] Makimono v2 (app tracking con Kitsu API)
