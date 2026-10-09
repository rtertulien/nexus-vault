---
title: Tracker Doble Hacha
date: 2026-08-26
tags: [games, stoneshard, tracker, html]
status: activo
type: project
up: "[[Projects MOC]]"
related: ["[[The Lawbringer]]"]
---

# Tracker Doble Hacha — Stoneshard Build Tracker

> Tracker de builds para Stoneshard (parche 0.9.4.25 Blood Omens). Primera build documentada: Doble Hachas Jorgrim. Prototipo tracker v3 en vanilla HTML. Segunda build: [[Blademaiden Arna]] (2H espada AGI).

## Estado

- **Activo.** Investigación completada, build Doble Hachas definida, prototipo tracker v3 creado.
- Proot `stoneshard-tracker-v3.md` creado con 5 fases (pendiente ejecutar con agentes).

## Build — Doble Hachas Jorgrim

| Atributo | Valor |
|---|---|
| STR | 25 |
| AGI | 20 |
| PER | 12 |
| VIT | 15 |
| Skills | 31 |

### Árboles de skills (31 total)
- **Axes:** 8
- **Dual Wield (DW):** 7
- **Survival:** 9
- **Athletics:** 5
- **Warfare:** Seize + Setup

## Gear

- **Armadura:** MEDIA — Captain set (~19.9k)
- **Hachas:** Baron + Knight / Aldwynn

## Progresión

- Investigador: top 10 builds (parche 0.9.4.25 Blood Omens)
- Staff híbrido = meta actual; hachas = variantes viables
- Wands: no lanzado aún
- Wiki Stoneshard: 1,980 págs; árboles de skills por Treatises
- 9 personajes jugables
- Bosses difíciles: Manticore, Queen, Marshal

## Codec — AntoKrator

- Formato: gzip + base64, URL ledger v1
- `formatVersion` attr, stats STR/AGI
- Usado para compartir/importar builds

## Tracker v3 — Grimdark Forge

- **Prototipo:** `stoneshard-tracker-v3/index.html` (vanilla)
- **Tema:** Grimdark Forge, acento `#8b0000`
- **Layout:** 6 bloques × 5, gap 14px
- **Nota:** violó stack premium y outcome loop (se hizo directo en main chat). Corregido con proot de 5 fases.

## Proot

- `stoneshard-tracker-v3.md` — 5 fases (pendiente ejecutar)

## Próximos pasos

- [ ] Ejecutar proot `stoneshard-tracker-v3.md` con agentes por fases
- [ ] Implementar tracker v3 con stack premium (no vanilla)
- [ ] Integrar codec AntoKrator (gzip+base64 ledger v1)
- [ ] Documentar más builds (staff híbrido meta, variantes hachas)

## Links

- Wiki: Stoneshard wiki (1,980 págs)
- Relacionado: [[The Lawbringer]] — build planner Stoneshard previo