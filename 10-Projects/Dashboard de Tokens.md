---
title: Dashboard de Tokens
type: project
status: activo
prioridad: alta
inicio: 2026-10-02
created: 2026-10-02
updated: 2026-10-07
area: "[[Dev]]"
up: "[[Projects MOC]]"
related: ["[[Alethe]]", "[[Agentic OS (descartado)]]", "[[Codex vs Claude Code]]"]
tags: [dashboard-tokens, codex, opencode, tokens, creditos, alethe, solo-lectura]
---

# Dashboard de Tokens

Dashboard de **solo lectura** de tokens y **créditos restantes** (diario / semanal / mensual) de **Codex + OpenCode**.

**Estado:** activo — brainstorming iniciado. Fuentes de datos confirmadas; falta cerrar diseño (deploy + pantallas) antes de implementar.

## Concepto

- El usuario trabaja con dos terminales en conjunto (OpenCode + Codex) y quiere ver de un vistazo cuánto consume cada uno y cuánto queda.
- **Solo lectura**: no ejecuta ni controla agentes, solo lee y muestra.

## Ejecutor asociado

[[Alethe]] será el **ejecutor** de agentes (PTY real, Codex + OpenCode). El dashboard **NO se integra con Alethe**: Alethe no expone API (es Tauri) y su formato interno no es público → el dashboard lee las **fuentes nativas** de cada herramienta (Codex es Codex, lo use quien lo use).

## Bloqueo abierto (antes de diseñar)

**RESUELTO (2 Oct 2026).** El recon local confirma que hay datos reales, no hace falta inventar.

### Fuentes confirmadas

**OpenCode** — `C:\Users\terrible019\.local\share\opencode\opencode.db` (SQLite, ~688 MB):
- Tabla **`session`** (424 filas): `cost`, `tokens_input`, `tokens_output`, `tokens_reasoning`, `tokens_cache_read` → consumo por sesión.
- Tabla `message` (12 898) y `part` (49 166) → detalle por mensaje (modelo/proveedor dentro de `data`).
- También existe `~/.local/share/opencode/storage/` y el `serve` (puerto 4096, `/session`).

**Codex** — `C:\Users\terrible019\.codex\`:
- `state_5.sqlite` → tabla **`threads`** (51 filas) con `tokens_used`, `model_provider`, `title`, `cwd`, `updated_at`.
- `goals_1.sqlite` → `thread_goals` con `token_budget` y `tokens_used`.
- `.codex-global-state.json` (330 KB) → **`codex-rate-limit-reset-history`** (límites reales) y `sidebar-usage-snapshot-attempts-v1`.
- `logs_2.sqlite` (46 846), `thread_history_1.sqlite`, `history.jsonl`, `session_index.jsonl`.
- `auth.json`: `auth_mode = "chatgpt"` → plan ChatGPT (cuota por ventana, no "créditos" numéricos).
- Versión Codex: `0.160.0`.

### Implicaciones

- **OpenCode**: "restante" NO es real de forma local → solo consumo/costo (o estimado contra presupuesto que defina el usuario).
- **Codex**: sí expone datos de **rate limit** (histórico de resets) → la parte de "límites reales" es viable.
- El dashboard leería SQLite + JSON en **modo solo lectura** (algunos con WAL activo → abrir con `mode=ro`).

## Decisiones tomadas (brainstorming 2 Oct 2026)

- **Objetivo nº1 (elegido por el usuario):** *Consumo + límites reales.* Mostrar consumo de tokens/costo Y los límites reales que expongan Codex/OpenCode. **Nada de presupuesto inventado.**
- Clasificación: **arquitectural** (proyecto nuevo). Proceso: brainstorming → spec → writing-plans → implementación por fases.

## Preguntas abiertas

- ¿Dónde corre? (web local / Tailscale / PWA) — **pendiente**.
- ¿Qué pantallas/vistas? (hero de consumo, desglose por herramienta/agente/modelo, línea de tiempo, límites).
- ¿Frecuencia de refresco (en vivo por SSE o solo al abrir).

## Antecedente

[[Agentic OS (descartado)]] intentó algo parecido (app + orquestador + ejecutor) y se descartó el 02 Oct 2026 por alcance. Este proyecto es deliberadamente **más pequeño**: solo lectura, sin orquestación, sin acoplarse a Alethe.

---

## Actualización — 7 Oct 2026 (stack final + hallazgo Antigravity)

**Stack final:** OpenCode (orquestación Orca + backend) · **Claude Code** (frontend/diseño, Opus 5.5 vía `@designer`) · **Antigravity** (verificación). **Codex se retiró** (CLI desinstalado; Desktop intacto). Ver [[Codex vs Claude Code]].

**Panel CUADRO:** diseño `handoff/panel-tokens/cuadro/final-cuadro.html`; backend real `handoff/panel-tokens/server/server.js` (Node zero-dep, puerto 8770). Datos REALES:
- **OpenCode**: cuota Rolling/Weekly/Monthly de `opencode.ai/console/api/go/status` (cookie de sesión de Helium → `opencode-session.txt`; `refresh-session.ps1` al caducar) + tokens/coste de `opencode.db`.
- **Claude Code**: ventanas de `api.anthropic.com/api/oauth/usage` + tokens/coste de `ccusage`.
- **Alertas por email reales** (Resend; `server/email.json`), evaluador cada 60 s con cooldown. `burn` = tasa real de actividad (0 si inactivo).

**Hallazgo Antigravity (`handoff/panel-tokens/agy/RESEARCH-ANTIGRAVITY.md`):** la cuota/uso de Antigravity **sí** tiene fuente real:
- **Tokens** (in/out/cache/modelo): `~/.gemini/antigravity/conversations/<id>.db` (+ `antigravity-cli/`), tabla `gen_metadata.data` (protobuf varint; f1.f4 → f2 input, f3/f10 output). **Extraíble offline** (~30 líneas).
- **Cuota remota %/reset**: ConnectRPC `RetrieveUserQuotaSummary` vía `language_server.exe` (127.0.0.1) con header `x-codeium-csrf-token` (efímero; en `ANTIGRAVITY_CSRF_TOKEN` solo para sidecars) → requiere **sidecar** con el IDE abierto.
- **Coste USD**: no lo da Google (suscripción); estimar con precios Gemini.

**Pendiente (mañana):** implementar en `server.js` el lector de tokens de Antigravity (`gen_metadata`) + coste estimado; decidir cuota % (sidecar) o `sin dato` honesto. Opcionales: histórico 60 d, Firefox, lanzador + README, email de mc-dashboard.
