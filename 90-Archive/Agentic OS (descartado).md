---
title: Agentic OS (descartado)
date: 2026-10-02
tags: [project, agentic-os, descartado, lecciones, dashboard-tokens]
status: archivado
updated: 2026-10-02
type: project
area: "[[Dev]]"
up: "[[Projects MOC]]"
related: ["[[Alethe]]", "[[Servidor Minecraft]]", "[[MC Dashboard]]"]
---

# Agentic OS (descartado — 02 Oct 2026)

> [!failure] Proyecto descartado y borrado
> Tras varias rondas de depuración, el usuario decidió **abandonarlo y borrar el código**. El problema no era un bug: era el **alcance** — se construyó como app + orquestador + ejecutor, y eso multiplicaba los puntos de fallo. El proyecto nuevo será **solo un dashboard de tokens** (Codex + OpenCode).

## Qué era

Centro de comando privado para OpenCode + homelab: sesiones, chat, orquestación, tareas, costo/tokens, backups, vault Nexus y terminal. 8 fases de proot, BFF Python stdlib + SQLite, frontend vanilla HTML/CSS/JS. 103 tests. Desplegado en `http://127.0.0.1:8797`.

## Por qué se descartó

- **Alcance demasiado grande.** Cada función nueva (chat, PTY, tareas, orquestación) añadía superficie de fallo.
- **Punto único de fallo:** todo dependía de `opencode serve`. Si se caía (y se cayó), **toda** la UI quedaba vacía.
- **La UI no sabía degradar:** `Promise.all` tumbaba todos los paneles con un solo fallo.
- El usuario lo resumió: *"es el proyecto más roto que hemos hecho, nada funciona"* — aunque el backend sí funcionaba.

## Bugs reales encontrados y arreglados (lecciones)

Esto es lo que **no hay que repetir** en el proyecto nuevo:

1. **SSE invisible en el navegador (HTTP/1.0).** El servidor usaba `protocol_version` por defecto (HTTP/1.0) y mandaba `Connection: keep-alive` **sin `Content-Length` ni `chunked`**. Un navegador no puede delimitar el cuerpo → el `EventSource` falla al instante. **Python sí lo leía** (un socket crudo ignora el framing) → por eso "en backend funciona y en el navegador no". Fix: HTTP/1.1 + `Transfer-Encoding: chunked`.
2. **Un 2xx puede ser una mentira.** El chat devolvía `202` mientras el upstream rechazaba. **Nunca reportar éxito sin comprobar la respuesta del upstream.**
3. **El formato del modelo importa.** El `serve` exige `{providerID, modelID}`, no `{id,...}`. Fallaba con `400 Missing key at ["model"]["modelID"]`.
4. **CSRF rotado en cada `/auth/me`** → todo POST daba 403 tras un fallo del SSE.
5. **Login rechazaba `Origin: localhost`** (solo `127.0.0.1`) → UI vacía sin mensaje útil.
6. **`Promise.all` → `Promise.allSettled`**: una UI debe mostrar lo que sí tiene, no apagarse entera.
7. **`/agents` y `/providers` sin try/except** → 500 en vez de 503 honesto.
8. **Los mocks de test no definían modelo de sesión** → el código debe ser tolerante, no crashear.

## Lo que SÍ funcionaba (y es reutilizable como conocimiento)

- **Lectura de tokens de OpenCode.** El `serve` (puerto 4096) expone `/session` con `cost`, `tokens{input,output,reasoning,cache}`, `agent`, `model`. Verificado en vivo: **100 sesiones, $9,28 total, 378M tokens**.
- **Backfill de usage** desde las sesiones del upstream (por agente, modelo, día, hora) con caché.
- **SSE** con HTTP/1.1 + chunked (patrón correcto documentado arriba).
- **BFF Python stdlib + SQLite** sirviendo UI same-origin con auth por cookie + CSRF.
- **Allowlist de orígenes** sin comodines (localhost/127.0.0.1/Tailscale, puerto exacto).

## El proyecto nuevo (decisión 02 Oct 2026)

**Dashboard de administración de tokens y límites**, solo lectura/monitor:

- **Fuentes:** OpenCode (`serve`, ya verificado) + **Codex** (archivos locales `~/.codex/` — por confirmar dónde guarda el uso).
- **Alethe** (`https://alethe-agents.kc1t.com/`, `github.com/Kc1t/alethe-agents`) será **el ejecutor** de agentes (PTY real, Codex + OpenCode). El dashboard **NO se integra con Alethe**: lee las fuentes nativas de cada herramienta (Codex es Codex, lo use quien lo use).
- **Alethe no expone API** (es Tauri) y su formato interno **no es público** → no acoplar el dashboard a él.
- **Pendiente antes de diseñar:** verificar dónde/cómo guarda **Codex** su uso (bloquea la viabilidad).

## URLs y rutas (por si se reconstruye algo)

- OpenCode upstream: `http://100.96.216.96:4096` (Basic, user `opencode`, pass en env de usuario `OPENCODE_SERVER_PASSWORD`)
- BFF viejo: `http://127.0.0.1:8797` · login `admin`/`admin`
- Diseño congelado: `Documents\chat de opencode\agentic-os-mockups\propuesta-17-definitiva.html`
- Proot viejo: `Documents\proot\proot-agentic-os.md`
