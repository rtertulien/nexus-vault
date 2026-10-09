---
title: Setup 3 IAs (OpenCode + Codex + Antigravity)
date: 2026-10-02
tags: [opencode, codex, antigravity, alethe, ai-memory, setup]
status: activo
type: project
up: "[[Projects MOC]]"
---

# Setup 3 IAs (OpenCode + Codex + Antigravity)

Operan en paneles sueltos de **Alethe** (`Kc1t/alethe-agents` v1.7.0), sin puente entre ellos.
El usuario es el único router.

## Roles

- **OpenCode** = centro de comando + backend dueño (decide, implementa `js/api.js` real, deploy).
- **Codex** = frontend (diseña backend-blind, mocks, `DESIGN.md` + `CONTRATO.md`).
- **Antigravity** = verificador + investigador + código rutinario (no edita archivos ajenos, reporta en artefactos).

## Reglas anti-colisión

1. 1 worktree/rama por agente. Nunca dos editando lo mismo.
2. Ownership explícito de rutas en cada BRIEF.
3. Handoff por archivos (`handoff/<feature>/`), no por chat entre agentes.
4. `handoff/<feature>/`: `BRIEF.md` (yo) → `codex/` + `agy/` (entregas) → `DECISION.md` (yo, CONTRATO final único).

## Modelos Antigravity (verificado foros + docs)

| Modelo | Uso |
|---|---|
| 3.8 Flash | workhorse (Terminal-bench 90.8%) |
| 3.1 Pro High | planes, arquitectura, research (SWE 80.6%) |
| 3.6 Flash | cambios medios |
| 3.7 Flash | evitar (reportes poco fiable) |
| Claude 4.6 | legacy y capado en Antigravity (thinking ~1k, output capado) — solo estilo Claude explícito |

Routing 70% Flash / 30% Pro. Plugin `opencode-antigravity-auth` DESCARTADO (viola ToS, baneos reportados).

## Skills + agentes Antigravity

5 skills (verificador, investigador, rutinas, routing, reporte) + 3 extra propias (token-saver, contract-validator, git-safe-ops) + 4 custom agents + reglas-duras always_on. Cero MCPs (Opción A). Viven en `C:\Users\terrible019\.gemini\antigravity\`.

## Memoria compartida (ai-memory)

- Binario `~/.local/bin/ai-memory.exe` v0.10.0 en PATH + `AI_MEMORY_DB` compartida.
- Alethe espera: binario `ai-memory` + `--version` + spec stdio `args: ["mcp"]` (fuente: `src-tauri/src/ai_memory.rs`).
- Codex cableado a mano en `~/.codex/config.toml` (ruta absoluta + `--db` + `RUST_LOG=off` + timeout 120). Fix real: logs por stdout rompían el handshake; además Codex no resuelve PATH al spawnear.
- Namespace por proyecto sin espacios: `chat-opencode`. Proots en `proot-memoria-codex.md` y `proot-memoria-antigravity.md`.
- Doc: `alethe-ai-memory.md` (carpeta chat de opencode).

## Archivos de operación (carpeta chat de opencode)

`brief-3ias.md`, `proot-memoria-*.md`, `alethe-ai-memory.md`, `handoff/_TEMPLATE/`, `prueba-fuego/` (referencia auditada conforme por Antigravity).

## Pendiente (hecho 2026-10-03)

Primer loop conjunto **prueba-latencia** PASS: Codex maquetó historial
(CONTRATO+mock+history.html con modos SUPERVISOR→SHINOBI), Antigravity
verificó 5/5 + regresión con capturas, OpenCode conectó adapter real a
`history.json` y cerró en `DECISION.md`. Cero colisiones.

## Plugins OpenCode (instalados manual, sin node en sistema)

autotitle 0.1.3, openslimedit 1.0.4, snip 2.0.0, token-tracker 1.7.1
(tarballs por shasum; `integrity` venía corrupto). opencode-mem fuera
(duplica ai-memory). Roster de agentes en `handoff/_TEMPLATE/agentes.md`.

## Orquestación Alethe

Existe (`alethe_delegate` + board) pero solo backends Claude/Codex.
Antigravity Fase 3 (0%), OpenCode Fase 4 (0%). Handoff por archivos sigue.
