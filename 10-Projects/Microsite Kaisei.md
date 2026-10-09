---
title: Microsite Kaisei
date: 2026-10-03
tags: [microsite, kaisei, frontend, 3-ias, orca, codex, antigravity, opencode, scroll-narrativo]
status: activo
type: project
up: "[[Projects MOC]]"
---

# Microsite Kaisei

> Micro-site editorial de lanzamiento para **"Kaisei"**, un cuaderno ficticio de pizarra mineral reutilizable (borra con agua). Scroll narrativo, arte japonés-moderno de taller.

## Proyecto

Pieza de producto inventado usada para probar el flujo 3-IA en serio, con subagentes reales y verificación visual. 5 secciones narrativas:

1. Hero no centrado (Bodoni Moda oversized + cuaderno en CSS/SVG).
2. Capítulo 1 — Origen (stage **pinned** con capas minerales y scrub).
3. Capítulo 2 — Oficio (4 hotspots interactivos con `aria-pressed` + `aria-live`).
4. Specs en scroll horizontal pinned (no tabla plana).
5. Reserva — CTA única ("Reserva la primera tirada").

## Flujo ejecutado

| Fase | Quién | Entrega | Veredicto |
|---|---|---|---|
| Diseño + construcción | Codex (`designer-max`) | `microsite/` (index.html, css/, js/, DESIGN.md, CONTRATO.md) | entregado |
| Verificación | Antigravity (`verificador`) con navegador Orca | `agy/VERIFY.md` + snapshots | PASS |
| QA adversarial | shinobi-reviewer | `review-shinobi.md` | 87/100 APROBADO |
| Fix loop | Codex | `js/app.js`, `index.html` | 4 fixes |
| Integración + cierre | OpenCode | `DECISION.md` + memoria + vault | OK |

## Anti-slop aplicado

- Paleta **crema / carbón / terracota**, fuera de la banda morado/índigo (`--paper #f1ede3`, `--ink #282c27`, `--clay #9b4935`).
- Pairing **Bodoni Moda** (display) + **Manrope** (cuerpo), no Inter para todo.
- Reveals variados por sección: clip-path, stroke-dash, scrub, stagger; ease propia `cubic-bezier(0.22, 1, 0.36, 1)`.
- Radius por rol: 0 estructura/diagramas, 2px CTA, 3–8px objeto cuaderno, 50% hotspots.
- Sin gradiente morado, hero centrado, 3 cards iguales, glassmorphism nav ni pill badges.

## Fix loop (shinobi 87/100)

1. **[MAYOR]** `body[data-motion='enhanced']` nunca se seteaba → doble scroll en desktop. Codex lo setea en `app.js:91`.
2. **[MENOR]** `innerHTML` con SVG → `insertAdjacentHTML`.
3. **[MENOR]** `retryButton.firstChild.textContent` frágil → `<span data-retry-label>`.
4. **[MENOR]** `.specs-viewport` sin `role` → `role="region"` (`index.html:102`).

## Código y handoff

- Código: `C:\Users\terrible019\Documents\chat de opencode\microsite\`
- Handoff: `C:\Users\terrible019\Documents\chat de opencode\handoff\microsite\` (`BRIEF.md`, `proot-codex.md`, `proot-agy.md`, `review-shinobi.md`, `DECISION.md`, `agy/VERIFY.md` + `agy/snapshots/`)

## Aprendizaje

- Los 3 agentes usan ya sus **subagentes reales** (Codex = `designer-max`, Antigravity = `verificador`).
- Antigravity usó el **navegador embebido de Orca** (`goto` / `screenshot` / `set-device`) para evidencia visual por estado y device.
- El runtime de Orca se cayó una vez a mitad (recuperar con `orca status`); workers "idle" dejan el run `in_progress` → hay que `worker-stop`.
- El flujo queda como plantilla reutilizable: BRIEF → proot-codex → proot-agy → shinobi → fix → DECISION.

## Relacionado

- [[Prueba orquesta]]
- [[Setup 3 IAs (OpenCode + Codex + Antigravity)]]
- [[Orquestador Orca]]
- [[Mimir]]
- [[Mimir - Protocolo anti-ruptura]]
- [[Codex Frontend y OpenCode Backend]]
