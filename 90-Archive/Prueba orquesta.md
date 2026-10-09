---
title: Prueba orquesta
date: 2026-10-03
tags: [3-ias, orquestacion, orca, prueba-orquesta, codex, antigravity, opencode, ai-memory]
status: archivado
type: project
up: "[[Projects MOC]]"
---

# Prueba orquesta

## Objetivo

Landing showcase de agentes con **3 cards** (OpenCode, Codex, Antigravity) para probar el flujo 3-IA de punta a punta bajo orquestación Orca. Cada card lista los agentes reales definidos en el BRIEF.

## Flujo probado

| Fase | Agente | Entrega |
|---|---|---|
| Diseño + construcción | Codex | `prueba-orquesta/` (index.html, css/styles.css, js/api.js, js/app.js, DESIGN.md, CONTRATO.md) |
| Verificación | Antigravity | `handoff/prueba-orquesta/agy/VERIFY.md` — PASS 5/5 |
| QA | shinobi-reviewer | `review-shinobi.md` — 78/100 RECHAZADO → fix loop → 90/100 PASS |
| Integración | OpenCode | adapter real + cierre |

## Criterios y resultado

- Frontend backend-blind: `index.html` no hace `fetch` directo; todo el I/O pasa por `api.getAgentes()` en `js/api.js`.
- 4 estados cableados y observables: loading / vacío / error / éxito.
- 3 cards con los agentes reales (OpenCode, Antigravity, y los de frontend de Codex).
- Antigravity `VERIFY.md` = PASS (5/5 criterios).
- shinobi-reviewer: primer pase 78/100 (accesibilidad 60 < 75) → fix loop (touch targets ≥44px, contraste, alineación error mock/contrato) → 90/100 PASS.
- Sin colisiones: Codex solo tocó `prueba-orquesta/`, Antigravity solo escribió en `agy/`, OpenCode integró sin tocar el diseño.

## Ubicación

- Código: `C:\Users\terrible019\Documents\chat de opencode\prueba-orquesta\`
- Handoff: `C:\Users\terrible019\Documents\chat de opencode\handoff\prueba-orquesta\` (`BRIEF.md`, `proot-codex.md`, `proot-agy.md`, `review-shinobi.md`, `agy/VERIFY.md`)

## Resultado

Flujo 3-IA con orquestación [[Orquestador Orca]] funcionando de punta a punta: Codex construye → Antigravity verifica → shinobi-reviewer hace QA → OpenCode integra. Memoria compartida **ai-memory** operativa: los 3 agentes guardan (namespace `chat-opencode`).

## Relacionado

- [[Setup 3 IAs (OpenCode + Codex + Antigravity)]]
- [[Codex Frontend y OpenCode Backend]]
- [[prueba-latencia]]
- [[Prueba fuego]]
- [[Mimir]]
- [[Mimir - Protocolo anti-ruptura]]
