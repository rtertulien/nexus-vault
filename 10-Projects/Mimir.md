---
title: Mimir
type: project
status: idea
prioridad: media
inicio: 2026-10-02
created: 2026-10-02
updated: 2026-10-02
area: "[[Dev]]"
up: "[[Projects MOC]]"
related: ["[[Alethe]]", "[[Dashboard de Tokens]]", "[[Codex Frontend y OpenCode Backend]]", "[[Mimir - Protocolo anti-ruptura]]"]
tags: [proyecto, agentes, tauri, alethe, mimir, fase-0]
---

# Mimir

> Workspace de escritorio de agentes CLI (estilo Alethe) para uso propio: corre OpenCode, Codex y Claude en PTY reales con paneles, y crecerá hasta igualar/superar a Alethe con funciones propias.

## Concepto
App de escritorio local-first, **Windows-first**, que ejecuta agentes CLI de IA en terminales reales (PTY) con paneles redimensionables, sub-tabs, gestión de MCP/skills y sistema de plugins. No es multiagente genérico ni un "context hub": es el workspace de agentes en sí, como Alethe, pero propio.

## Estado
**Aparcado — Fase 0 (preparación).** No se escribe código de la app todavía. Se retoma cuando el usuario esté listo.

## Decisiones tomadas (2026-10-02)
- **Construir desde cero, por fases** (NO fork de Alethe). Alethe se usa solo como **plano de referencia** (copiar decisiones, no código).
- **Windows-first**; macOS después.
- Agentes iniciales: **OpenCode + Codex**, luego Claude (no los 14 de Alethe).
- **MVP usable en ~4-6 semanas** de trabajo efectivo; no se apunta a paridad completa de entrada.
- No se distribuye → sin firma de código, sin notarización, sin i18n, sin perfiles múltiples.

## Por qué desde cero (y no fork)
Alethe es **AGPL-3.0**: para uso propio sin distribuir, forkear es legal y no obliga a publicar. Pero heredar ~108k LOC de Rust+React con deuda reconocida es inmantenible si no las escribiste. Construir desde cero da propiedad y comprensión; el riesgo de estancamiento se mitiga con MVP temprano por fases.

## Análisis de Alethe (medido, 2026-10-02)
- ~108.000 LOC estimadas (TS ~64k, Rust ~30k, CSS ~12k), 895 archivos, 15+ subsistemas.
- Stack: Tauri 2 + Rust + React 18 + Zustand + xterm.js + portable-pty. 14 agentes CLI.
- Sistema de plugins RFC-012 (catálogo con SHA-256, scheme `alethe-plugin://`), MCP manager, git/worktrees, orquestador (orchestrator_core.rs 93 KB + 2º binario), remote control (servidor Rust + cliente web 61 KB), voz on-device (sherpa-onnx), i18n 340 KB.
- 9 releases en 3,5 meses (jun→sep 2026), 409 commits, 793★, 1 autor principal (58% commits) con IA intensiva.
- Deuda: ~9% cobertura tests frontend, 24/50 módulos Rust sin tests, god-object de 3.080 líneas, duplicación. Windows sin firmar (Defender lo marca), macOS sin notarizar.

## Estimación de esfuerzo (meses-persona)
- **MVP** (PTY + 3-4 agentes + persistencia + layout): 200–400 h → 1–2 meses.
- **Paridad con Alethe 1.7.0**: 1.200–2.500 h → 6–10 meses (1 dev) · 4–6 con 2.
- **Superarlo**: 2.500–4.000+ h → 12–18+ meses.
- Lo commodity: xterm.js, portable-pty, Tauri, Zustand, paneles. Lo caro: PTY nativo cross-platform (Ghostty FFI macOS, ConPTY Windows, kill de process groups Linux), adaptadores por agente, plugin system, cliente remoto, orquestación, empaquetado.

## Mercado (competidores)
Nicho saturado: Conductor (Mac-only), Nimbalyst (ex-Crystal, Crystal deprecado), Vibe Kanban, Terragon (shutdown feb-2026), Omnara (mobile nativo), AgentAPI, Sculptor (containers), Cursor Agents, Warp, Zed Terminal Threads, Termany, Orkestrai, Athena, Terra Swarm, Maestro/RunMaestro, claude-squad, Claude Flow, container-use, Goose, Crush, Aider. Dos murieron en 2026 → riesgo de mercado.

## Fase 0 — Preparación (antes de escribir código)
1. Documentación profunda de Alethe: arquitectura, plugins, orquestador, docs/DIAGNOSTICO_MATURIDADE_TECNICA.md, issues/discussions/foros, releases.
2. Leer documentación completa de Codex (CLI, plugins, skills, MCP, hooks, AGENTS.md) y de OpenCode (serve API 4096, plugins, agents, MCP).
3. Aprender el toolchain: instalar Rust + Node, entender cómo se compila/empaqueta un `.exe` (Tauri build, NSIS/MSI). Firma: no necesaria para uso propio.
4. Decidir stack definitivo: **Tauri+Rust vs Electron+Node** (pendiente).
5. Definir alcance del MVP y roadmap hasta paridad.
6. Escribir spec + proot.

## Visor propio de agentes (nueva dirección)
El usuario quiere una **página propia** para "ver a los agentes trabajando y llegando a un objetivo" (no traces genéricos ni herramienta de terceros). Se probó Arize Phoenix y se descartó (el plugin OTel de opencode no resuelve `@grpc/grpc-js`). Este visor pasa a ser una pieza de Mimir. Proot: `proyectos-opencode\_docs\proot\proot-visor-agentes.md`. Sin implementar hasta retomar.

## Guardrails
Ver [[Mimir - Protocolo anti-ruptura]] — protocolo anti-ruptura de agentes (gates, control de contexto, QA con revisor distinto, permisos deny-by-default) que servirá de base para el `AGENTS.md` y `docs/` del repo Mimir.

La suite ya está **endurecida** (2026-10-02): AGENTS.md recortado (982 → 183 líneas), denylist de permisos en `opencode.jsonc` y `steps` por subagente. Ver [[Suite de agentes - Endurecimiento 2026-10-02]].

## Pendiente / próximo paso
- Cerrar la decisión de stack (punto 4).
- Cuando el usuario retome, ejecutar Fase 0 completa antes de poner a OpenCode/Codex a programar.
- Visor de agentes de Mimir: base de estudio en [[Ver agentes (OpenCode + Codex) sin terminal]] (gctrl vs Langfuse vs Phoenix; aparcado hasta que el usuario avise).
- **Proots ya creados** en `proyectos-opencode\_docs\proot\`: `proot-mimir.md` y `proot-visor-agentes.md`.

## Referencias
- Alethe: https://github.com/Kc1t/alethe-agents
- Se usa el agente `investigador` para Alethe/Codex/OpenCode y `content-curator` para el mercado.
