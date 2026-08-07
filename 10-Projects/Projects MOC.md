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
- [[Shinobi Track]]
- [[Makimono]]
- [[VeraCode]]
- [[Coda]]
- [[GLITCH HUD]]
- [[Tally]]
- [[Proyecto Humanitario]]
- [[The Lawbringer]]

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
