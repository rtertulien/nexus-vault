---
title: "Alethe"
type: resource
status: activo
up: "[[Nexus]]"
date: 2026-10-02
related: ["[[Dashboard de Tokens]]", "[[Codex vs Claude Code]]"]
tags:
  - alethe
  - alethe-agents
  - ejecutor-agentes
  - opencode
  - codex
  - tauri
  - windows
---

# Alethe

**Alethe — "The multi-agent coding workspace".** Workspace de escritorio local-first que corre agentes CLI y shells en **PTYs reales** dentro de un layout persistente por proyecto.

- **Repo:** `github.com/Kc1t/alethe-agents` · **Web:** `https://alethe-agents.kc1t.com/`
- **Stack:** Tauri 2 + Rust + React 18/TS/Vite + Zustand + xterm.js + portable-pty.
- **Versión / licencia:** v1.7.0 (20-sep-2026) · AGPL-3.0-or-later.
- **Nota:** NO es un orquestador (es un multiplexor/workspace); la orquestación es un módulo opcional apagado por defecto.
- **Windows** es la plataforma más probada.

## Agentes soportados

`claude`, `codex`, `copilot`, `cursor`, `antigravity`, `opencode`, `kiro`, `kimi`, `grok`, `codewhale`, `mimo`, `freebuff`, **Shell** (pwsh/cmd), **WSL**.

- Con **session resume**: Claude Code, Codex, Cursor, OpenCode.
- Con **usage cards**: Claude, Codex, Antigravity.

## Configuraciones recomendadas (usuario: Windows, OpenCode + Codex)

- `enabledAgents` → solo **OpenCode + Codex + Shell**.
- `cliPaths` → solo si un agente sale "not found" (override manual al exe/cmd).
- `spawnConcurrency` → **3** (bajar a 2 si hay congelos al abrir grupos).
- `notifyOnLimitReset` → **ON** (avisa al resetear la ventana de uso de Claude/Codex).
- `resourcePolicy.mode` → **manual** ("Monitor only"). NUNCA `smart-lru` ni `automaticParkingOptIn`.
- **Features OFF:** `aiMemory`, `playwright`, `orchestrator`, `gsdSync`.
- **Features ON:** `mcp`, `graphify`, `prs`, `browser`.
- Terminal command **`alethe`** instalado (`%LOCALAPPDATA%\Alethe\bin\alethe.cmd`).
- `9router` OFF. Voice dictation OFF.
- Remote Control opcional con **`remoteUseTailscale: true`** (LAN plano va sin TLS).
- Notificaciones de "agente terminó": **nativas SIN toggle** → aceptar el permiso de Windows y revisar "No molestar".

## Gotchas

- **Defender** marca el exe como `Trojan:Win32:Bearfoos.A!ml` → Security → Protection history → **Restore** + excluir `%LOCALAPPDATA%\Alethe`.
- Cierre lento: hasta **4 s** con agentes corriendo (issue #275, abierto).
- Cuelgue de ventana en Windows 11 reportado (issue #271).
- `orchestrator`: crash duro `0xc0000409` si un worker devuelve >16.000 bytes con acentos/emoji (issue #256) → motivo extra para no activarlo.
- Update-check **siempre ON**, sin preferencia para desactivarlo.
- El **polling de uso** (Claude/Codex/Antigravity cada 5 min) sigue activo aunque ocultes el indicador.
- Secretos (Spotify, 9router) guardados **en claro** en `projects.json` → tratar los backups como sensibles.
- `spawn.log` y `telemetry.jsonl` no rotan.

## Vacíos (verificar en la app)

- Ruta exacta de datos en Windows: `%LOCALAPPDATA%\Alethe` vs `com.kc1t.alethe` (inconsistencia).
- Valor efectivo de Discord Rich Presence (el código dice OFF; la doc de privacidad dice ON).

## Relación con proyectos

Es el **ejecutor de agentes** del próximo proyecto [[Dashboard de Tokens]] (que será solo lectura y NO se integra con Alethe).

## Sesión Codex CLI — 2026-10-02

- **Codex CLI instalado:** paquete completo oficial, versión **0.160.0**, en `C:\Users\terrible019\AppData\Local\Programs\OpenAI\Codex\bin\codex.exe`. El instalador inicial de PowerShell falló por `OSArchitecture`; se usó PowerShell 7 y el instalador oficial.
- **Verificación:** `codex --version` informó 0.160.0 y `codex login status` confirmó sesión iniciada mediante ChatGPT. No guardar credenciales en notas.
- **Alethe:** el usuario configuró Codex y confirmó que funciona; la terminal respondió a `hola` en español.
- **Avisos pendientes:** Codex mostró `Hook failed` (código 1), causa aún no investigada. El aviso del launcher Windows sobre “embedded mode” apareció al iniciar sin servidor compartido; no impidió que la TUI respondiera.
- **Contexto de trabajo:** la terminal mostraba `~\AppData\Local\Alethe`; antes de pedir cambios de código, abrir o crear el proyecto con la carpeta del repositorio deseado como directorio de trabajo.
- **Objetivo de uso:** replicar el espacio de cuatro terminales (2×2) del demo de Alethe, con Codex y OpenCode. Alethe ofrece layout automático y configuración de cuadrícula; aún falta dejarlo configurado y verificado en el proyecto correcto.
