---
title: Bug - Orca no detecta agy (Antigravity v1.2.16)
date: 2026-10-03
tags: [orca, antigravity, agy, bug, orquestacion, windows]
status: active
type: resource
up: "[[Orquestador Orca]]"
---

# Bug: Orca no detecta el TUI de agy (Antigravity CLI v1.2.16)

## Síntoma

Al lanzar Antigravity como worker supervisado (`orca orchestration worker-start --agent antigravity`),
Orca falla en `agent_readiness` (timeout) o el worker queda `in_progress` sin cerrar.
También `orca terminal wait --for tui-idle` da **timeout** con agy.

## Evidencia (2026-10-03)

- `worker-start` → `state=failed, stage=agent_readiness`.
- Preview del terminal mostraba "Welcome to the Antigravity CLI. You are currently not signed in. Signing in..." — el sign-in del OAuth tarda y Orca no lo reconoce.
- Aun logueado, `tui-idle` → `error: timeout`.
- **La tarea SÍ se ejecuta**: leyendo el terminal, agy respondió correctamente ("AGY-OK-2").
- `fallbackReason: provider_unsupported` al leer su transcript.

## Causa raíz

Orca espera marcadores concretos del TUI de agy para readiness/idle, y **agy v1.2.16 no los emite igual**. Es el mismo patrón de incompatibilidad que sufrió Codex 0.157 (que Orca arregló en v1.4.218).

**No es mal estado de la máquina ni configuración del usuario**: es la versión de agy adelantada al soporte de Orca.

## Impacto

- Antigravity ejecuta bien, pero Orca no sabe cuándo está listo ni cuándo terminó.
- Los workers de agy quedan en timeout/in_progress → hay que `worker-stop` a mano.
- Codex SÍ funciona (su versión ya está soportada).

## Workarounds

1. **No orquestar agy** como worker supervisado; usarlo en un terminal normal de Orca (o fuera) y recoger su salida a mano.
2. Reintentar `worker-start` (a veces el timing coincide y arranca).
3. Esperar update de Orca que soporte agy v1.2.16.
4. Cerrar a mano: `orca orchestration worker-stop --dispatch <id>`.

## Relacionado

- [[Orca - Protocolo Windows (gotchas)]]
- [[Orquestador Orca]]
