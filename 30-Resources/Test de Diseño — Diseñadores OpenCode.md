---
title: Test de Diseño — Diseñadores OpenCode
date: 2026-09-26
type: resource
status: activo
tags: [recurso, diseño, agentes, codex]
up: "[[Resources MOC]]"
related: ["[[MC Dashboard]]", "[[Codex vs Claude Code]]", "[[Agentes]]", "[[Premium Design Stack]]"]
---

# Test de Diseño — Diseñadores OpenCode

> Prueba comparativa de los agentes de diseño (`designer-max` vs `designer-lite`) sobre un **brief congelado**. Objetivo: medir disciplina de tokens, calidad de markup y honestidad de estados.

## Prueba

- **Brief congelado:** MC Dashboard (`BRIEF.md`).
- **Carpeta:** `C:\Users\terrible019\Documents\design-test\`
- **Salidas:** `BRIEF.md` + `max/` + `lite/` + `max-pixel/`

## Resultados

### designer-max = GPT-6 Sol — GANADOR
- 14 KB, 200 líneas, **16 variables**, **10 hex**, editorial minimal.
- Markup semántico, contraste **AA OK**, botón deshabilitado **honesto**.
- Responsive hasta 480 px.

### designer-lite = GPT-6 Luna
- 11.9 KB pero **MINIFICADO** (69 líneas) → malo para editar después.
- Más denso / dev-tool.
- `min-width: 1100px` → **rompe en pantalla chica**.
- Botón vivo **falso** (no refleja el estado real).

### max-pixel = Sol con el estilo pixel 8-bit Minecraft
- Reutiliza el estilo del proyecto existente [[MC Dashboard]].
- 267 líneas, 16 KB, **23 variables**, **14 hex**, **1 keyframe**, AA mínimo **7.7:1**.
- Usuario: *"en verdad me gusta el diseño"*.
- **Perdió dos cosas del original:**
  - El **bisel** (borde claro arriba-izq / oscuro abajo-der = sensación de bloque 3D).
  - Los **widgets de juego** (barras HP/MP, HUD creeper).
- Conservó el **esqueleto** (borde bloque, sombra dura, marca creeper, cifras mono, acentos XP/HP/MP/gold) pero **no el alma**.

## Comparación con el proyecto existente

| | Sol (max) | mc-dashboard existente |
|---|---|---|
| Tamaño | 14 KB | 101 KB |
| Líneas | 200 | 1901 |
| Hex | 10 | 197 |
| Ancho mín. | 480 px | 500 px |
| Animaciones | — | 23 (12 keyframes) |
| Backend | — | `server.py` 29 KB |

- **Sol:** disciplina brutal (14 hex vs 197).
- **Existente:** mucha **identidad**, poca disciplina.

## Conclusión

- **Fusión ideal:** tokens de **Sol** + **bisel** y **barras** del original.
- **Decisión:** el proyecto [[MC Dashboard]] **ya está hecho, no se toca**; queda como **referencia para un futuro rebranding**.

---

## Fix de entorno

### Problema: skills fantasma en OpenCode
- El plugin **`opencode-power-pack`** tenía la **caché VACÍA** (`C:\Users\terrible019\.cache\opencode\packages\opencode-power-pack@git+https_` → solo carpetas, cero archivos) → por eso fallaba *"Skill frontend-design not found"*.
- Además **`taste-skill`**, **`emil-animation`** y **`landing-patterns`** **NO existían** en ningún lado: eran **nombres fantasma** escritos en los prompts de `designer` / `designer-max`.

### Solución: instalar las 15 skills reales
- No hay `git` en el equipo → se bajó el repo con `Invoke-WebRequest` del **zip de GitHub**.
- Instaladas en `C:\Users\terrible019\.config\opencode\skill\`:
  - `frontend-design`, `ai-slop`, `design-patterns`, `code-architect`, `code-explorer`, `code-quality`, `code-review`, `code-reviewer`, `security-review`, `security-threat-model`, `feature-dev`, `mcp-builder`, `skill-creator`, `agents-md-improver`, `agents-md-revise`.
- **Mapeo de los fantasmas:**
  - `taste-skill` → **`ai-slop`**
  - `emil-animation` → **`ui-ux-pro-max`** `data/motion.csv`
  - `landing-patterns` → **`ui-ux-pro-max`** `data/landing.csv`
- Se **quitó el plugin roto** de la lista `plugin` en `opencode.jsonc`.
- **Requiere reiniciar OpenCode** para que cargue las skills.

### Lección
> No confiar en el índice de skills; **verificar que el `SKILL.md` exista en disco**.

---

## Enlaces
- Relacionado: [[MC Dashboard]] · [[Codex vs Claude Code]] · [[Agentes]] · [[Premium Design Stack]]
- Hub: [[Resources MOC]] · [[Nexus]]
