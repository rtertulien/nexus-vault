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

- [[Coda]]
- [[Tally|Tally — Expense Tracker]]
- [[Mac Setup 2026]]
- [[Shinobi Track]]
- [[Makimono]]
- [[VeraCode]]
- [[Proyecto Humanitario]]
- [[The Lawbringer]]
- [[Homelab - Mini PCs]]
- [[Homelab - Docker Compose]]
- [[Dead Again - Game Design|Dead Again — Zombie Incremental]]
- [[FitTrack - App Design|FitTrack — Fitness App]]

## 📚 Recursos

- [[PixelLab]] — Sprites con IA

## 🎯 Hoy

```tasks
not done
due before tomorrow
limit 5
```
