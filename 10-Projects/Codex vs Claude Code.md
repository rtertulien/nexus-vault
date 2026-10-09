---
title: Codex vs Claude Code vs opencode-go
date: 2026-09-24
updated: 2026-09-27
tags: [project, agentes, codex, claude-code, opencode, comparativa, arquitectura, diseño, decision, codex-setup, gpt-6]
status: activo
type: project
priority: alta
area: "[[Dev]]"
up: "[[Projects MOC]]"
related: ["[[30-Resources/Agentes]]", "[[30-Resources/Plugins opencode]]", "[[Mac Setup 2026]]"]
---

# Codex vs Claude Code vs opencode-go

> Investigación 24 Sep 2026 · comparativa de los tres ecosistemas de agentes para **diseño y arquitectura**. La decisión es qué suscripción de $20 pagar como motor de trabajo, sin romper las reglas de uso.

## Decisión

> **Cerrada el 26 Sep 2026:** **ChatGPT Plus es el entorno principal** (app full: chat + Codex integrados). **OpenCode se queda para el volumen** (agentes del sistema, iteración barata, MCP/plugins).

**Codex Plus ($20) — elegido.**

- Más horas de uso que la competencia.
- Acceso a la familia GPT-6 (Astra / Sol / Luna).
- **Figma bidireccional** (lee y escribe el archivo de diseño).
- **Integración por API legal** dentro de [[30-Resources/Plugins opencode|OpenCode]] — sin OAuth, sin riesgo de ban.

## Claude Pro ($20)

- **Opus 5.5** como modelo por defecto.
- Fuerte en **arquitectura** (razonamiento largo, decisiones de diseño de sistema).
- **Cupo corto** — se agota rápido para sesiones de trabajo reales.
- **NO integrar por OAuth**: usar la suscripción dentro de otro cliente vía OAuth viola los términos → riesgo de **ban** (referencias: **PR #18186**, **issue #6930**).

## Codex — modelos

| Modelo | Precio (in / out por 1M) | Nota |
|---|---|---|
| **GPT-6 Astra** | $10 / $50 | Tope de la familia · **#1 en WebDev** · cupo Plus 5-45 msgs/5h · **solo pase final** |
| **GPT-6 Sol** | $2 / $10 | **#4 en WebDev** · el **max diario** de trabajo |
| **GPT-6 Luna** | $0.10 / $0.50 | **157 t/s** · el lite (tweaks, volumen) |
| GPT-5.6 | — | Legacy (Sol / Terra / Luna) — el que usan hoy varios agentes del vault |
| GPT-5.5 | — | Legacy |
| ~~GPT-6 Terra~~ | — | **No existe** |

### Regla de uso

- **Astra** → pase final / arquitectura. **1 tarea dura al día** (el cupo Plus es 5-45 msgs/5h).
- **Sol** → build / implementación. Es el máximo diario.
- **Luna** → tweaks, iteraciones y volumen.

## opencode-go

- **32 modelos documentados / 42 disponibles por API.**
- **Top diseño:** **Qwen3.8 Max**.
- **Top arquitectura:** **Kimi K3** / **GLM-5.3**.
- Es el proveedor que ya corre los 7 agentes del sistema (ver [[30-Resources/Agentes|Agentes]]).

## Skills y plugins

- **Claude gana en diseño:** su skill **Frontend Design** acumula **1.13M** de uso.
- **Codex gana en orquestación de empresa** (workflows, equipos, integraciones).
- **OpenCode es el más flexible** — control total de agentes, MCP y plugins.

### Top plugins de Codex
**Figma** · **Superpowers** · **Stark** · **memi** · **UIZZE** · **Archcore** · **Cloudflare**

## Ranking de diseño (actualizado)

- Claude (Frontend Design) sigue liderando en diseño.
- Codex sube en orquestación y en el modelo **GPT-6 Sol (#4 WebDev)**.
- opencode-go mantiene el mejor coste/flexibilidad con **Qwen3.8 Max** para diseño y **Kimi K3 / GLM-5.3** para arquitectura.

## Riesgo de ban — postura del usuario

- **El usuario NO quiere ban.**
- Vía elegida: **API key legal**.
- Si se quiere usar Claude en paralelo, hacerlo **sin fusionar el OAuth** (cuentas/proveedores separados, nunca conectar la suscripción al cliente de terceros).

## Setup del 26 Sep 2026

Sesión de configuración: **Codex Plus como principal + OpenCode para volumen**.

### Agentes en OpenCode

- **`designer-max`** → `openai/gpt-6-sol` (diseño fuerte, coste bajo).
- **`designer-lite`** → `openai/gpt-6-luna` (iteración barata, 157 t/s).
- **Alternativa opencode-go:** `qwen3.8-max` ($2 / $6, **#1 en Frontend Code**) como max y `opencode-go/gpt-6-luna` como lite.

### Anti-ban — postura firme

- **NUNCA** usar el **OAuth de la suscripción Anthropic** dentro de OpenCode (viola términos → ban). Referencias: **PR #18186**, **issue #6930**.
- **OpenAI OAuth está tolerado** (es el camino ya conectado), pero **sin contrato** — puede cambiar. El fallback legal sigue siendo **API key**.

### Tuning de Codex CLI

- `/fast off` → ahorra **~2x** cuota.
- `tool_output_token_limit = 8000`.
- `agents.max_threads = 2`.
- **AGENTS.md lean** (contexto mínimo).
- Effort **low / medium**.

### Fixes aplicados

- **`config.toml` con duplicados** (`enabled` y `memories` repetidos) **rompía el TOML** → corregido.
- **~320 skills excedían el budget del 2% / 8000 chars** → **desactivados** `universal-design-principles`, `vercel` y `openai-templates`; **`token-saver` duplicado** → se deja **solo** en `~/.agents/skills`.

### Plugins de Codex

**Stark** · **memi** (Memoire) · **UIZZE** · **Archcore** · **Figma** · **Superpowers** · **GitHub** · **Vercel** · **Cloudflare** · **Codex Security** · Documents/PDF · Spreadsheets · Presentations · Template Creator · Browser · Computer Use · Visualize.

## Pendiente

- [x] Contratar **Codex Plus** y conectar su API en [[30-Resources/Plugins opencode|OpenCode]].
- [ ] Evaluar **GPT-6 Sol / Luna** como reemplazo de GPT-5.6 en los agentes.
- [ ] Probar **Qwen3.8 Max** (diseño) y **Kimi K3 / GLM-5.3** (arquitectura) contra el stack actual.

## Sesión 27 Sep 2026 — Setup OpenCode → Codex

- **Dump completo del setup OpenCode** entregado (opencode.jsonc: 8 plugins, 4 MCP, 3 providers, 7 agentes + modelos).
- **Creado `AGENTS-codex.md`** en `Documents\chat de opencode\` — fusión de los 7 prompts + reglas + stack + rubrics en 10 secciones. NOTA: línea "Codex no tiene subagentes" quedó desactualizada.
- **Verificado:** Codex SÍ tiene subagentes (custom agents TOML en `~/.codex/agents/` o `.codex/agents/`, built-ins `default`/`worker`/`explorer`, `max_depth` default 1). Los 7 agentes portan 1:1 como TOML.
- **Plan externo de setup Codex (1 mes) evaluado:** acertado en recortar global a 1 página, quitar scores numéricos, no exigir DESIGN.md+proot en fixes pequeños, semana de prueba antes de pagar. Matiz: no disolver los 7 agentes en skills — usarlos como custom agents TOML solo en features grandes.
- **Prompt bootstrap para chat Codex** entregado (reglas + flujo + diseño + stack preferido).
- **Trial Plus 1 mes gratis:** promo real (`chatgpt.com/promo_campaign=plus-1-month-free`, confirmada India/EEUU, otras regiones según cuenta). Requiere tarjeta, auto-renueva $20/mes → cancelar 1 semana antes. Oferta estudiantes EEUU (4 meses, SheerID, hasta 31 oct 2026) NO aplica (uni RD).
- **Pendiente:** recortar global a 1 página + crear TOML (planner, reviewer, researcher, designer) en `~/.codex/agents/`; probar en repo no crítico durante el trial.

## Enlaces
- Relacionado: [[30-Resources/Agentes|Agentes]] · [[30-Resources/Plugins opencode|Plugins opencode]] · [[OpenCode + Xcode Setup]] · [[Mac Setup 2026]]
- Hub: [[Projects MOC]] · [[Nexus]]
