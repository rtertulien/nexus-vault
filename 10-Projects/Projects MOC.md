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
- [[Shinobi Track]] — PWA seguimiento anime, desplegado
- [[Makimono]] — PWA lector manga, proot listo
- [[Coda]] — Subscription tracker, desplegado
- [[GLITCH HUD]] — Gaming news landing page, completado
- [[Isaac Guide]] — Wiki The Binding of Isaac, pausado
- [[Spotify Wrapped]] — Year in review, completado
- [[VeraCode]] — Videojuego forense, spec listo
- [[Mac Setup 2026]] — Setup G14 + MacBook + iPhone
- [[Homelab - Mini PCs]] — Homelab setup
- [[Homelab - Docker Compose]] — Docker services
- [[Proyecto Humanitario]] — Centro madres solteras
- [[The Lawbringer]] — Build planner Stoneshard
- [[iOS Projects]] — Apps nativas iOS
- [[Desarrollo Android]] — Apps Android
- [[Dead Again - Game Design]] — Game design document
- [[FitTrack - App Design]] — Fitness app design

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
WHERE type = "project" AND contains(list("idea","en-pausa","activo"), status)
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
FROM "10-Projects"
WHERE type = "project" AND status = "archived"
```
