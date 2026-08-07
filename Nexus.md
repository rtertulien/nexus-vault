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
WHERE type = "project" AND status != "archived"
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

- [[GLITCH HUD]] — Gaming news landing page
- [[Shinobi Track]] — PWA seguimiento anime
- [[Makimono]] — PWA lector manga
- [[Coda]] — Subscription tracker
- [[Isaac Guide]] — Wiki The Binding of Isaac
- [[Spotify Wrapped]] — Year in review
- [[VeraCode]] — Videojuego forense
- [[Mac Setup 2026]] — Setup G14 + MacBook + iPhone
- [[Homelab - Mini PCs]] — Homelab setup
- [[Homelab - Docker Compose]] — Docker services
- [[Proyecto Humanitario]] — Centro madres solteras
- [[The Lawbringer]] — Build planner Stoneshard
- [[iOS Projects]] — Apps nativas iOS
- [[Desarrollo Android]] — Apps Android
- [[Dead Again - Game Design|Dead Again — Zombie Incremental]]
- [[FitTrack - App Design|FitTrack — Fitness App]]

## 📚 Recursos

- [[30-Resources/Premium Design Stack|Premium Design Stack]] — Stack completo de diseño premium (GSAP, Three.js, Lenis, APIs)
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
