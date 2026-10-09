---
title: Orquestador Orca
date: 2026-10-03
tags: [orquestacion, orca, agentes, opencode, codex, antigravity]
status: active
type: resource
up: "[[Resources MOC]]"
---

# Orquestador Orca

Orca es el orquestador de agentes CLI del setup 3-IA. MIT, Windows nativo, v1.4.219.
Corre OpenCode, Codex y Antigravity en paneles/worktrees y añade una capa de
orquestación (Runs, Tasks, Dispatches, `worker_done`).

## Qué permite

- Lanzar una oleada de workers y esperar sin vigilar: `run-create` → `worker-start`
  → `check --wait` → `reply/ack` → `worker-release`.
- Coordinator = rol: el terminal Orca vinculado a un Run. OpenCode lo ejerce.
- `orca-cli` (skill): navegador embebido por worktree (`goto`, `snapshot`, `click`,
  `fill`, `screenshot`, `set device`).
- **Design Mode** (función de la app): click en un elemento → HTML + CSS + screenshot
  al terminal del agente.

## Script

`handoff/orquestador-orca/orquestar.ps1` lanza Codex + Antigravity, resuelve el Enter
auto de Codex (bug de paste, issue #11343) y espera los `worker_done`.

## Fix de subagentes de Codex

- Causa raíz: `AGENTS-codex.md` negaba los subagentes. Codex SÍ los tiene en
  `~/.codex/agents/*.toml`.
- Activos: `designer-max`, `shinobi-reviewer`, `investigador` (Codex es frontend).
  Los otros 5 quedaron en `_removed_*.toml`.
- `[agents]` en `config.toml`: `default_subagent_model = "gpt-6.1-sol"`, cap 3.

## Relacionado

- [[Microsite Kaisei]]
- [[Prueba orquesta]]
- [[Setup 3 IAs (OpenCode + Codex + Antigravity)]]
- [[Mimir]]
