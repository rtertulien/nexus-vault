---
type: spec
project: Coda
status: completed
fecha: 2026-06-23
up: "[[Coda]]"
area: "[[Dev]]"
tags: [spec, coda, subscriptions, fintech, implemented, deployed, v1]
url: https://coda-3j8.pages.dev
deploy_hash: a2e79b9e
---

# Coda — Subscription Management PWA

> Enterprise-grade subscription tracker
> Implementado: 23 Jun 2026 en `C:\\Users\\rtert\\Documents\\coda\\`
> Desplegado: https://coda-3j8.pages.dev (https://4e0245bf.coda-3j8.pages.dev)
> 10 archivos, 31 bugs corregidos en 3 rondas de revision

## Logo
- **Híbrido B + A:** Barras decrecientes + dots laterales
- SVG puro, minimalista, sin texto
- Colores: barras verdes `#10b981` con opacidad decreciente (0.9 → 0.4)
- Archivo: `C:\Users\rtert\Documents\Kobo\coda-logo-final.svg`

## Implementación
- **Fecha:** 23 Jun 2026
- **Archivos:** 10 (HTML, CSS, 7 JS, SW, manifest, icons)
- **Stack:** HTML+CSS+JS vanilla, IndexedDB, Service Worker, Cloudflare Pages
- **Bugs corregidos:** 32 en 4 rondas (shinobi-reviewer + supervisor)
- **URL:** [coda-3j8.pages.dev](https://coda-3j8.pages.dev)
- **Ruta local:** `C:\Users\rtert\Documents\coda\`

## Changelog

### 23 Jun 2026 — v1.0
- Implementación completa: 10 archivos, HTML+CSS+JS, IndexedDB, SW
- **Ronda 1 (designer + shinobi):** 3 bugs (formatNextDue rollover, creadaEn perdido al editar, autocomplete position)
- **Ronda 2 (shinobi + supervisor):** 16 bugs (XSS, formatNextDue today/invalid, DB conexiones, import race, SW skipWaiting, notificaciones, metodo pago rediseño)
- **Ronda 3 (shinobi):** 7 bugs (Object.assign antes de await, activa al editar, notificaciones sin clamp, import string prices, days 999, lista edit undefined, polling)
- **Ronda 4 (shinobi + supervisor):** 8 bugs (XSS color/categoria, historial anual/trimestral, formatNextDue NaN, notification gesture iOS, delete try/catch, import no atomico, nombre vacio)
- **Ronda 5 (shinobi):** 1 bug (XSS diaCobro en historial)
- **Deploy:** https://coda-3j8.pages.dev (hash 8be529ca)

### 23 Jun 2026 — Fix campana + snackbar
- Bell icon autocomplete conflict: closeSheet() now blurs shName and closes autocomplete to prevent iOS refocus issue after notification permission dialog
- requestNotificationPermission now shows snackbar on grant
- Snackbar CSS bug: translateY(100px) left it visible at viewport edge (bottom:100px + translateY(100px) = not enough to hide). Changed to opacity fade in/out
- Final deploy hash: a2e79b9e

## Agentes
- **investigador:** Pre-build: top 20 servicios populares + precios + fechas típicas + viabilidad PWA notificaciones iOS
- **designer:** CSS premium, iconos SVG, layouts, paleta corporativa
- **data-ninja:** IndexedDB, export/import JSON, lógica de cálculo
- **shinobi-reviewer:** QA, edge cases, PWA, offline, responsive
- **supervisor:** Revisión general, estructura

## Stack
HTML + CSS + JS vanilla, IndexedDB, Notification API, SW, CF Pages

## Paleta corporativa
- Fondo `#0a0a0a`, superficie `#111111`, cards `#f1f5f9`
- Acento verde `#10b981`, azul hielo `#60a5fa`, plateado `#e2e8f0`

## Tabs (4)
1. **Dashboard** — próximas facturas + gasto mensual/anual + categorías
2. **+ (FAB)** — modal añadir suscripción
3. **Lista** — todas categorizadas, plegables, subtotales
4. **Ajustes** — métodos de pago, export/import, notificaciones

## Features clave
- Scroll horizontal próximas facturas con snap
- Precio anual automático junto al mensual
- Método de pago por suscripción (Visa, Mastercard, Qik, PayPal, etc.)
- Categorías con barra de progreso y porcentaje
- Agrupación expandible con subtotal por categoría
- Export/import JSON completo
- Notificaciones al abrir app si hay cobros hoy (sin backend, PWA offline)
- Badge en icono con count de cobros próximos (Badging API)
- Sin emojis, iconos SVG premium, diseño corporativo

## Coste estimado
3-5h implementación

## Links
- Spec completo: `C:\Users\rtert\Documents\Kobo\specs\2026-06-16-coda-design.md`

