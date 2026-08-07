---
type: spec
project: Makimono
status: design-validated
fecha: 2026-06-16
prioridad: alta (julio)
up: "[[Makimono]]"
area: "[[Dev]]"
tags: [spec, makimono, kitsu, design]
---

# Makimono v2 — Design Spec

> Seguimiento de mangas + descubrimiento
> Pendiente implementación: Julio 2026

---

## Stack
- HTML + CSS + JS vanilla
- Kitsu API (manga)
- IndexedDB (biblioteca offline)
- CF Pages + Functions (proxy OAuth)
- PWA
- Diseño oscuro premium con animaciones 10/10

## Paleta
| Rol | Hex |
|---|---|
| Fondo | `#0a0a1a` |
| Superficie | `#1a1a2e` |
| Elevada | `#252540` |
| Cards | `#f1f5f9` |
| Acento | `#60a5fa` (azul hielo) |
| Estrellas/badges | `#cbd5e1` |

## Tabs (4)
1. **Mi Lista** — Leyendo, Planeados, Completados, Abandonados
2. **Tendencias** — Top populares
3. **Buscar** — Búsqueda por título
4. **Mix** — Recomendaciones aleatorias

## CSS Premium
- Glassmorphism en sheets y tab bar
- Spring animations (`cubic-bezier(0.32, 0.72, 0, 1)`)
- Ripple effect en cards/botones
- Stagger fade-in en grids (IntersectionObserver)
- Shimmer parallax en skeletons
- View Transitions API entre tabs

## Arquitectura
- Clone de Shinobi Track (probado en iOS)
- `app.js`, `kitsu.js`, `engine.js`, `storage.js`, `auth.js`
- Login Kitsu via proxy CF Functions
- Bottom sheet glassmorphism con swipe-to-close
- Grid 2 cols, cards 70×98px con badge de formato

## Coste estimado
3-4h implementación

## Links
- Spec completo: `C:\Users\rtert\Documents\Kobo\specs\2026-06-16-makimono-v2-design.md`
- Proot: `C:\Users\rtert\Documents\proot\makimono-v2-tracking.md`
