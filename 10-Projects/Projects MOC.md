---
type: moc
tags: [moc]
up: "[[Nexus]]"
related: ["[[Dev]]", "[[00-Inbox]]"]
---

# Projects MOC 🎯

> Hub de proyectos. Cada nota de proyecto enlaza aquí.

> Template: [[30-Resources/Project Template|📋 Project Template]]

## Proyectos
- [[Dashboard de Tokens]] — Dashboard de solo lectura de tokens/créditos Codex + OpenCode, idea
- [[Mimir]] — Workspace de escritorio de agentes CLI (estilo Alethe), aparcado en Fase 0
- [[Shinobi Track]] — PWA seguimiento anime, desplegado
- [[Makimono]] — PWA lector manga, proot listo
- [[Coda]] — Subscription tracker, desplegado
- [[GLITCH HUD]] — Gaming news landing page, completado
- [[Check Diario]] — Check diario de 6 cuentas, web de escritorio, completado
- [[Isaac Guide]] — Wiki The Binding of Isaac, pausado
- [[Spotify Wrapped]] — Year in review, completado
- [[VeraCode]] — Videojuego forense, spec listo
- [[Mac Setup 2026]] — Setup G14 + MacBook + iPhone
- [[Setup Laptop Gamer 2026]] — MSI Raider A18 HX, research compra
- [[Homelab - Mini PCs]] — Homelab setup
- [[Homelab - Docker Compose]] — Docker services
- [[Servidor Minecraft]] — Servidor MC con amigos, completado
- [[MC Dashboard]] — Dashboard web en tiempo real del servidor MC, planeado
- [[Proyecto Humanitario]] — Centro madres solteras
- [[The Lawbringer]] — Build planner Stoneshard
- [[Tracker Doble Hacha]] — Build tracker Stoneshard, activo
- [[Blademaiden Arna]] — Build 2H espada AGI Stoneshard
- [[iOS Projects]] — Apps nativas iOS
- [[Desarrollo Android]] — Apps Android
- [[Dead Again - Game Design]] — Game design document
- [[FitTrack - App Design]] — Fitness app design
- [[Codex vs Claude Code]] — Codex Plus = entorno principal, OpenCode = volumen (setup GPT-6)

## Active

```dataview
TABLE
  priority AS "Pri",
  due AS "Due",
  stack AS "Stack"
FROM "10-Projects"
WHERE type = "project" AND status = "activo"
SORT priority DESC
```

## In Progress

```dataview
TABLE
  priority AS "Pri",
  status AS "Status"
FROM "10-Projects"
WHERE type = "project" AND contains(list("idea","pausado","activo"), status)
SORT priority DESC
```

## Done

```dataview
LIST
FROM "10-Projects"
WHERE type = "project" AND status = "completado"
```

## Archived

```dataview
LIST
FROM "90-Archive"
```
