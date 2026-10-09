---
title: Isaac Guide
up: "[[Projects MOC]]"
tags: [isaac, guia, referencia, gaming]
status: pausado
fecha_inicio: 2026-07-24
type: project
---

# Isaac Guide

Guía de referencia de The Binding of Isaac: habilidades, objetos, sinergias y datos del juego.

## Ubicación

C:\Users\rtert\Documents\isaac-guide\

## Stack

- HTML + CSS + JS vanilla
- CSS sprites para personajes
- Parser Python (Lua → JSON) para extraer datos del mod EID

## Diseño

- Fondo: #080808
- Acento rojo sangre: #8b0000
- Púrpura: #2d1b4e
- Fuente: Press Start 2P (retro pixel)

## Características

### 3 pestañas principales

| Pestaña | Descripción |
|---------|-------------|
| **Guía** | Acordeones con información general, objetos clave, transformaciones, secretos |
| **Personajes** | 34 personajes con CSS portraits, 5 categorías, modal con stats detalladas |
| **Items** | 1057 items con búsqueda y filtros por tipo, calidad, personaje |

### Base de datos

Parser Python que extrae nombres y descripciones del mod **EID** (External Item Descriptions) archivado en Lua, convertido a JSON:

- 721 collectibles
- 188 trinkets
- 97 cartas
- 51 pastillas

### Sprites

4 spritesheets extraídos de **PlatinumGod**:

| Spritesheet | Contenido |
|-------------|-----------|
| 
e-itm | Repentance items |
| bn-itm | Afterbirth+ items |
| pn-itm | Afterbirth items |
| 
ep | Repentance adicional |

- **Total:** 827 sprites en SPRITE_MAP
- **Collectibles:** 699/721 (97%)
- **Trinkets:** 127/188 (68%)
- **Cartas:** 0/97 (0%)
- **Pastillas:** 0/51 (0%)

### Sprites de personajes

- PlatinumGod no tiene sprites de personajes — solo items/trinkets
- Alternativa: Fandom wiki tiene iconos individuales (17 base + 17 Tainted)
- URLs: Character_{Name}_icon.png y Tainted_{Name}_icon.png
- Estrategia: descargar 34 imágenes individuales → compilar spritesheet con Python
- Los portraits CSS actuales son placeholder funcional

### Tier list (S)

12 objetos categoría S: D6, Cuchillo de Mamá, Azufre, Sagrado Corazón, Deidad, Fondo Rocoso, Mega Seta, Certificado de Defunción, Revelación, Cesárea, Corona Glitcheada, Dado Reductor.

Tiers A-D distribuidos, resto categoría F.

## Estructura de archivos

`
isaac-guide/
├── index.html
├── css/style.css
├── js/
│   ├── app.js
│   └── characters.js
├── data/
│   ├── data.js
│   ├── items.json
│   ├── sprites.js
│   ├── sprites.json
│   ├── parse_lua.py
│   ├── parse_css.py
│   ├── names_spa.lua
│   ├── spa.lua
│   ├── spa_rep.lua
│   └── en_us_full.lua
└── images/
    └── (4 spritesheets PNG)
`

## Sesión 2 — 24 Jul 2026 (completada)

- **Plugin instalado:** `opencode-power-pack` (11 skills)
- **7 agentes actualizados** con skills del power pack
- **Proot:** `C:\Users\rtert\Documents\proot\isaac-guide-sesion2.md`

### Fases ejecutadas

| Fase | Agente | Resultado |
|------|--------|-----------|
| 1 | investigador | Spritesheets accesibles, wiki Fandom confirmada, 721 collectibles es el set completo |
| 2 | data-ninja | 718 items matcheados con ratings reales de PlatinumGod, tiers redistribuidos |
| 3 | designer | CSS rediseñado ~732→1490 líneas, tema oscuro Isaac, scanlines CRT, glowing, pixel art |
| 4 | data-ninja | 34/34 PNGs descargados de Fandom, characters.js actualizado con campo `img` |
| 5 | shinobi-reviewer | 0 críticos, 3 medios (fixeados: unescape deprecated, dead CSS, .char-img faltante), 5 menores |
| 6 | — | Esta actualización del vault |

### Cambios concretos

- **Tiers reales PlatinumGod:** S:56, A:257, B:360, C:294, D:80, F:10 (antes: 12 S definidos a mano, resto F)
- **CSS:** ~1490 líneas, scanlines CRT, glows neón, pixel art aesthetic, responsive
- **Characters:** 34 PNGs reales en `images/characters/`, 17 color themes hover por personaje
- **Bugfixes:** `unescape`→`TextEncoder/TextDecoder`, ~120 líneas CSS muertas eliminadas, regla `.char-img` añadida

### Archivos clave

```
isaac-guide/
├── index.html          (293 líneas, 3 tabs)
├── css/style.css       (~1490 líneas)
├── js/
│   ├── app.js          (158 líneas)
│   └── characters.js   (~114 líneas, 34 chars con img)
├── data/
│   └── data.js         (1057 items con tiers reales PlatinumGod)
└── images/characters/  (34 PNGs)
```

### Estado actual post-sesión 2

| Recurso | Cantidad | Cobertura sprites |
|---------|----------|-------------------|
| Collectibles | 721 | 699/721 (97%) |
| Trinkets | 188 | 127/188 (68%) |
| Cartas | 97 | 0/97 (0%) |
| Pastillas | 51 | 0/51 (0%) |
| Personajes | 34 | **34/34 (100%)** — PNGs reales |
| Tiers | 1,057 | **718 items con rating real PlatinumGod** |

### Pendiente (próxima sesión)

- [ ] Cards/pills sprites desde PlatinumGod
- [ ] Test visual en navegador
- [ ] Deploy a Cloudflare Pages si se desea

## Notas

- Los tiers son provisorios y basados en experiencia personal + comunidad
- Los datos se actualizan vía parser Python ejecutado manualmente contra los archivos Lua del mod EID
- Integración futura con API oficial de PlatinumGod si está disponible
