---
title: Nexus
date: 2026-06-23
tags: [home, nexus, hub]
status: active
type: home
---

# Nexus 🏠

```dataview
TABLE
  status AS "Estado",
  prioridad AS "Pri",
  inicio AS "Inicio"
FROM "10-Projects"
WHERE type = "project" AND status != "archivado"
SORT prioridad DESC, status ASC
```

## 📅 Últimas dailies

```dataview
LIST
FROM "20-Daily"
SORT fecha DESC
LIMIT 7
```

## 🔗 Hub

[[Projects MOC|🎯 Proyectos]] · [[Dev|🛠️ Dev]] · [[Daily|🗓️ Daily]] · [[00-Inbox|📥 Inbox]] · [[30-Resources/Convenciones|📋 Convenciones]] · [[30-Resources/Make Your Own|💡 Make Your Own]]

## 📁 Proyectos

- [[Dashboard de Tokens]] — Dashboard de solo lectura de tokens/créditos Codex + OpenCode
- [[Mimir]] — Workspace de escritorio de agentes CLI (estilo Alethe), aparcado en Fase 0
- [[GLITCH HUD]] — Gaming news landing page
- [[Shinobi Track]] — PWA seguimiento anime
- [[Makimono]] — PWA lector manga
- [[Coda]] — Subscription tracker
- [[Check Diario]] — Check diario de 6 cuentas, web de escritorio
- [[Isaac Guide]] — Wiki The Binding of Isaac
- [[Spotify Wrapped]] — Year in review
- [[VeraCode]] — Videojuego forense
- [[Mac Setup 2026]] — Setup G14 + MacBook + iPhone
- [[Homelab - Mini PCs]] — Homelab setup
- [[Homelab - Docker Compose]] — Docker services
- [[Servidor Minecraft]] — Servidor MC con amigos (Tailscale + playit.gg)
- [[MC Dashboard]] — Dashboard web en tiempo real del servidor MC (planeado)
- [[Proyecto Humanitario]] — Centro madres solteras
- [[The Lawbringer]] — Build planner Stoneshard
- [[Tracker Doble Hacha]] — Build tracker Stoneshard
- [[iOS Projects]] — Apps nativas iOS
- [[Desarrollo Android]] — Apps Android
- [[Dead Again - Game Design|Dead Again — Zombie Incremental]]
- [[FitTrack - App Design|FitTrack — Fitness App]]
- [[Codex vs Claude Code|Codex vs Claude Code]] — Codex Plus = entorno principal, OpenCode = volumen

## 📚 Recursos

- [[30-Resources/Premium Design Stack|Premium Design Stack]] — Stack completo de diseño premium (GSAP, Three.js, Lenis, APIs)
- [[30-Resources/Test de Diseño — Diseñadores OpenCode|Test de Diseño — Diseñadores OpenCode]] — Comparativa de diseñadores (Sol vs Luna) + fix de skills
- [[30-Resources/Plugins opencode|Plugins opencode]] — Plugins, MCP servers y agentes configurados
- [[30-Resources/Agentes|Agentes]] — Sistema de 7 agentes opencode
- [[Obsidian Curator - Instrucciones]] — Mantenimiento del vault
- [[30-Resources/Optimización Tokens|Optimización de Tokens]] — Guía de ahorro
- [[PixelLab]] — Sprites con IA

## 🎯 Hoy

```tasks
not done
due before tomorrow
limit 5
```
