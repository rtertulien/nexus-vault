---
title: "Ver agentes (OpenCode + Codex) sin terminal"
type: resource
status: pausado
created: 2026-10-02
updated: 2026-10-02
up: "[[Resources MOC]]"
related: ["[[Mimir]]", "[[Suite de agentes - Endurecimiento 2026-10-02]]"]
tags: [agentes, monitoreo, opencode, codex, gctrl, observabilidad]
---

# Ver agentes (OpenCode + Codex) sin terminal

> Objetivo: ver OpenCode y Codex trabajando CON SUS SUBAGENTES en una vista, fuera de la terminal (se ve feo). Estado: **aparcado — el usuario avisará cuándo montarlo.** El orquestador de Alethe no sirve (es interno a Alethe).

## Hallazgo clave
No existe una GUI de terceros que lea las sesiones locales de OpenCode **y** Codex a la vez. Las rutas son:
- **`gctrl`** (Ground Control): única que lee AMBAS DBs locales (OpenCode + Codex), sin instrumentar, con **árbol de subagentes**. Pero es **TUI** (terminal).
- **Langfuse / Arize Phoenix / SigNoz**: dashboard **web unificado**, pero vía **OTel** (hay que configurar export en los dos agentes). Langfuse pide Docker.
- **OpenChamber / CodeNomad** (OpenCode) y **codex-webui / open-codex-ui** (Codex): GUIs web pero **por agente**, no integran.

## Opciones (tabla)
| Opción | Ve ambos | Fuera de terminal | Setup | Requiere |
|---|---|---|---|---|
| `opencode web` (nativo OpenCode) | No | Sí | Cero | Nada |
| App de Codex / ChatGPT desktop | No | Sí | Cero | ChatGPT |
| **gctrl** | **Sí** | No (TUI) | 5 min | Node |
| OpenChamber / CodeNomad | No (OpenCode) | Sí | Bajo | Node |
| codex-webui / open-codex-ui | No (Codex) | Sí | Bajo | Node |
| **Arize Phoenix** | **Sí** | Sí | Medio | Python + OTel |
| **Langfuse** (self-host) | **Sí** | Sí | Alto | Docker + OTel |

## Superficies verificadas (2026-10-02)
- **OpenCode**: `opencode web` (UI web nativa, sesiones + estado de servidores; subagentes = sesiones hijas, no panel dedicado confirmado). `opencode serve` puerto 4096, OpenAPI en `/doc`, **SSE en `/event` y `/global/event`**; subagentes vía `GET /session/:id/children` y `POST /session` con `parentID`. SDK oficial `@opencode-ai/sdk` (`session.children`, `event.subscribe()`). `experimental.openTelemetry` **roto en `opencode run`** (issue #13438) → usar plugin comunitario `@devtheops/opencode-plugin-otel`.
- **Codex**: `codex app-server` (JSON-RPC stdio/ws), primitivas Thread→Turn→Item; subagentes = threads con `parentThreadId`/`ancestorThreadId`. OTel opt-in en `~/.codex/config.toml` (`[otel]`, catálogo `codex.*`). Transcripciones locales en `~/.codex/sessions/**/rollout-*.jsonl` con parent-child. UI oficial (app desktop / web / IDE) muestra panel de subagentes.

## Recomendación
1. **Ahora (cero instalación):** `opencode web` + app de escritorio de Codex.
2. **Unificado rápido:** instalar **Node** (se necesita igual para Mimir) y usar **gctrl** (`npx gctrl`) → ambos agentes + árbol de subagentes. TUI.
3. **Dashboard web maduro:** **Langfuse** self-host (MIT, la compró ClickHouse) — requiere Docker + OTel.

## Setup Phoenix (2026-10-02) — DESCARTADO
Se montó Arize Phoenix y se conectó Codex (OTel en config.toml) y OpenCode (plugin comunitario `@devtheops/opencode-plugin-otel@1`). **No funcionó para OpenCode**: el loader de opencode no resuelve las dependencias del plugin (`Cannot find module '@grpc/grpc-js' from ''`), persistente tras varios reinicios; probar `http/protobuf` tampoco (no llegó a probarse en limpio). El usuario decidió **descartarlo**. Se hizo limpieza total:
- Borrada la carpeta `C:\Users\terrible019\phoenix` (venv + logs + scripts) y el auto-arranque `Startup\phoenix-otel.cmd`.
- Revertido `[otel]` en `~/.codex/config.toml` (restaurado desde `config.toml.bak-phoenix-2026-10-02`).
- Quitado el plugin del array `plugin` en `opencode.jsonc` (los permisos y `steps` se conservan; backup original `opencode.jsonc.bak-2026-10-02`).

## Desinstalación (si no gusta)
1. Borrar la carpeta `C:\Users\terrible019\phoenix` (contiene todo: venv + logs).
2. Restaurar `~/.codex/config.toml` desde `config.toml.bak-phoenix-2026-10-02` (o borrar el bloque `[otel]`).
3. Quitar la entrada `["@devtheops/opencode-plugin-otel@1", {...}]` del array `plugin` en `opencode.jsonc` (o restaurar desde `opencode.jsonc.bak-2026-10-02`).
4. Cerrar el proceso de Phoenix.

## Prerrequisitos actuales del equipo (2026-10-02)
- Node/npm: **no instalado** (no está en PATH).
- Docker: **no instalado**.
- Python: **sí** (permite Phoenix sin Docker).

## Pendiente (actualizado) — nueva dirección
El usuario quiere **una página propia** para "ver a los agentes trabajando y llegando a un objetivo". Esto pasa a ser **parte de [[Mimir]]** (no una herramienta de terceros). Alcance a definir: vista de agentes/subagentes en vivo orientada a objetivo (no traces genéricos). Guardar como requisito del futuro visor de Mimir; sin implementación hasta retomar el proyecto.
