---
title: Desarrollo Android — Setup y MCP Servers
date: 2026-07-20
status: activo
priority: media
type: project
tags:
  - project
  - android
  - desarrollo
  - movil
  - android-studio
  - mcp
up: "[[Projects MOC]]"
---

# Desarrollo Android

## Setup (HP Victus)

**Stack:** OpenCode + Android Studio (ACP integration)

### MCP servers para Android

| Server | Stars | Tools |
|--------|-------|-------|
| `Android-Studio-MCP-Claude-Code` | 2★ | 73 tools (el más completo) |
| `adbfriend` | 73★ | 18+ tools ADB |
| `gradle-mcp` | 55★ | builds, testing, dependencias |
| `android-tools-mcp` | 23★ | 20 tools Android (logcat, screenshot, deploy, Gradle) |

### Integración OpenCode + Android Studio

- ACP (Agent Client Protocol) desde Android Studio 2025.2+
- Configurar en AI Agent settings: command `opencode.exe`, args `acp`
- OpenCode aparece como agente en el AI Chat panel de Android Studio

### Proceso

1. Usuario instala Android Studio + SDK
2. Yo configuro opencode.json, MCP servers, agente custom, permisos
3. OpenCode ejecuta gradlew/adb via tool bash
4. Testing en emulador o Android viejo via USB

### Nota

Plan D original era solo Apple (MacBook Pro M5 Pro + iPhone 17 + AirPods). Se añadió desarrollo Android como complemento usando la HP Victus existente.

---

## Changelog

- 2026-07-20: Nota creada al extraer sección de [[Mac Setup 2026]]
