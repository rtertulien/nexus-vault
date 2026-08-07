---
title: "Dead Again"
type: project
status: idea
platform: iOS
framework: SpriteKit
up: "[[Nexus]]"
date: 2026-07-10
tags:
  - game
  - ios
  - incremental
  - zombie
  - spritekit
  - pixellab
---

# Dead Again — Incremental Zombie Apocalypse

> Juego incremental estilo Vampire Survivors. Movimiento manual, ataques automáticos. Mueres, mejoras, repites.

---

## Concepto

Roguelike incremental adictivo. Sobrevives oleadas de zombis, recoges loot, mueres, mejoras permanentemente, vuelves más fuerte.

## Game Loop

```
EXPLORAR → MATAR ZOMBIS → RECOGER LOOT → MORIR → MEJORAR PERMANENTEMENTE → REPETIR
```

## Control (estilo Vampire Survivors)

| Elemento | Control | Ejemplo |
|---|---|---|
| **Movimiento** | Manual (WASD/flechas/d-pad) | Mueves al survivor por el mapa |
| **Ataques** | Automático | Las armas disparan solas según cooldown |
| **Upgrades** | Elección del jugador | Al subir de nivel, eliges 1 de 3 opciones |
| **Loot** | Automático al tocar | Recoges al pasar por encima |

**Referencia:** Vampire Survivors, Brotato, HoloCure, 20 Minutes Till Dawn — todos usan este patrón.

## El Mundo

- **Mapa:** Ciudad destruida con zonas (centro comercial, hospital, fábrica, bunkers)
- **Progresión:** desbloqueas zonas al llegar a cierto nivel
- **Eventos aleatorios:** emboscadas, convoyes de suministros, bosses

## Stats Base

| Stat | Efecto |
|---|---|
| ❤️ Vida | Puntos de vida |
| ⚔️ Daño | Daño por ataque |
| 🛡️ Defensa | Reducción de daño |
| 🏃 Velocidad | Movimiento |
| 🎯 Precisión | Golpe exacto |

## Qué pierdes al morir

- ❌ Armas equipadas
- ❌ Objetos en inventario
- ❌ Progreso del mapa

## Qué conservas

- ✅ Stats base mejorados
- ✅ Habilidades desbloqueadas
- ✅ Zonas descubiertas
- ✅ Logros

## Sistema de Mejoras

### Tier 1 — Stats base (monedas de sangre 🩸)

| Mejora | Costo | Efecto |
|---|---|---|
| Fuerza +1 | 10 🩸 | +5 daño |
| Resistencia +1 | 10 🩸 | +10 vida |
| Aguante +1 | 15 🩸 | +1 stamina |
| Puntería +1 | 12 🩸 | +3% precisión |
| Velocidad +1 | 14 🩸 | +5% velocidad |

### Tier 2 — Habilidades (monedas de almas 💀)

| Habilidad | Costo | Efecto |
|---|---|---|
| Doble salto | 50 💀 | Saltar 2 veces |
| Dash | 75 💀 | Deslizarte rápido |
| Carga | 100 💀 | Ataque fuerte al correr |
| Regeneración | 120 💀 | +1 vida/5s |
| Doble loot | 200 💀 | 2x objetos al recoger |

### Tier 3 — Permanent perks (gemas 💎)

| Perk | Costo | Efecto |
|---|---|---|
| Segunda oportunidad | 1 💎 | Revivir 1 vez por run |
| Arma inicial | 2 💎 | Empezar con pistola |
| Mapa del tesoro | 3 💎 | Ver items en el minimapa |
| Veterano | 5 💎 | +50% XP siempre |

## Monedas

| Moneda | Se obtiene | Se usa en |
|---|---|---|
| 🩸 Sangre | Matar zombis | Stats base |
| 💀 Almas | Matar jefes | Habilidades |
| 💎 Gemas | Completar desafíos | Perks permanentes |

---

## SPRITES — Asset Pack Completo (PixelLab API)

### Personaje Principal

| Asset | Endpoint | Costo |
|---|---|---|
| Survivor 8 direcciones | `create-character-v3` | $0.041 |
| Walk animation (8 dirs) | `animate-character` | $0.032 |
| Idle animation | `animate-character` | $0.032 |
| Attack animation | `animate-character` | $0.032 |
| Death animation | `animate-character` | $0.032 |
| **Subtotal** | | **$0.169** |

### Enemigos (5 tipos de zombis)

| Asset | Endpoint | Costo |
|---|---|---|
| Zombie básico (4 dirs) | `create-character` | $0.012 |
| Zombie rápido (4 dirs) | `create-character` | $0.012 |
| Zombie tanque (4 dirs) | `create-character` | $0.012 |
| Zombie volador (4 dirs) | `create-character` | $0.012 |
| Zombie boss (8 dirs) | `create-character-v3` | $0.041 |
| Walk animation × 5 | `animate-character` | $0.065 |
| Attack animation × 5 | `animate-character` | $0.065 |
| Death animation × 5 | `animate-character` | $0.065 |
| **Subtotal** | | **$0.286** |

### Armas (6 tipos)

| Asset | Endpoint | Costo |
|---|---|---|
| Baseball bat | `create-image-pixflux` | $0.008 |
| Pistola | `create-image-pixflux` | $0.008 |
| Escopeta | `create-image-pixflux` | $0.008 |
| Cuchillo | `create-image-pixflux` | $0.008 |
| Rifle | `create-image-pixflux` | $0.008 |
| Hacha | `create-image-pixflux` | $0.008 |
| **Subtotal** | | **$0.048** |

### Habilidades/Power-ups (8)

| Asset | Endpoint | Costo |
|---|---|---|
| Spinning attack | `create-image-pixflux` | $0.008 |
| Dash/roll | `create-image-pixflux` | $0.008 |
| Rage mode | `create-image-pixflux` | $0.008 |
| Heal | `create-image-pixflux` | $0.008 |
| Shield | `create-image-pixflux` | $0.008 |
| Poison cloud | `create-image-pixflux` | $0.008 |
| Lightning | `create-image-pixflux` | $0.008 |
| Bomb | `create-image-pixflux` | $0.008 |
| **Subtotal** | | **$0.064** |

### Cofres y Loot (6)

| Asset | Endpoint | Costo |
|---|---|---|
| Cofre cerrado | `create-image-pixflux` | $0.008 |
| Cofre abierto | `create-image-pixflux` | $0.008 |
| Cofre dorado (raro) | `create-image-pixflux` | $0.008 |
| Moneda sangre | `create-image-pixflux` | $0.008 |
| Alma (esfera) | `create-image-pixflux` | $0.008 |
| Gema | `create-image-pixflux` | $0.008 |
| **Subtotal** | | **$0.048** |

### Consumibles (6)

| Asset | Endpoint | Costo |
|---|---|---|
| Botiquín | `create-image-pixflux` | $0.008 |
| Mediquímico | `create-image-pixflux` | $0.008 |
| Granada | `create-image-pixflux` | $0.008 |
| Cerveza (buff) | `create-image-pixflux` | $0.008 |
| Estrella (invencibilidad) | `create-image-pixflux` | $0.008 |
| Speed boots | `create-image-pixflux` | $0.008 |
| **Subtotal** | | **$0.048** |

### Tilesets (3 mapas)

| Asset | Endpoint | Costo |
|---|---|---|
| Ciudad destruida | `create-tileset` | $0.010 |
| Bosque oscuro | `create-tileset` | $0.010 |
| Interior abandonado | `create-tileset` | $0.010 |
| **Subtotal** | | **$0.030** |

### Objetos del mapa (12)

| Asset | Endpoint | Costo |
|---|---|---|
| Auto abandonado | `create-map-object` | $0.010 |
| Caja de madera | `create-map-object` | $0.010 |
| Barril | `create-map-object` | $0.010 |
| Escombros | `create-map-object` | $0.010 |
| Farola | `create-map-object` | $0.010 |
| Mesa rota | `create-map-object` | $0.010 |
| Silla | `create-map-object` | $0.010 |
| Contenedor | `create-map-object` | $0.010 |
| Valla | `create-map-object` | $0.010 |
| Poste | `create-map-object` | $0.010 |
| Puerta | `create-map-object` | $0.010 |
| Escalera | `create-map-object` | $0.010 |
| **Subtotal** | | **$0.120** |

### UI

| Asset | Endpoint | Costo |
|---|---|---|
| Health bar | `generate-ui-v2` | $0.095 |
| Botón play | `generate-ui-v2` | $0.095 |
| Panel upgrade | `generate-ui-v2` | $0.095 |
| Panel inventory | `generate-ui-v2` | $0.095 |
| Slot de arma | `generate-ui-v2` | $0.095 |
| **Subtotal** | | **$0.475** |

### Fondos (4)

| Asset | Endpoint | Costo |
|---|---|---|
| Ciudad destruida bg | `create-image-pixflux` | $0.010 |
| Bosque bg | `create-image-pixflux` | $0.010 |
| Interior bg | `create-image-pixflux` | $0.010 |
| Menu bg | `create-image-pixflux` | $0.010 |
| **Subtotal** | | **$0.040** |

### Efectos de partículas (5)

| Asset | Endpoint | Costo |
|---|---|---|
| Blood splash | `create-image-pixflux` | $0.008 |
| Soul collect | `create-image-pixflux` | $0.008 |
| Explosion | `create-image-pixflux` | $0.008 |
| Heal effect | `create-image-pixflux` | $0.008 |
| Level up | `create-image-pixflux` | $0.008 |
| **Subtotal** | | **$0.040** |

---

## SPRITES FALTANTES (análisis vs estándar del género)

| Categoría | Tienes | Necesitas | Faltan |
|---|---|---|---|
| Zombis | 5 | 13-17 | 8-12 tipos + 3-5 bosses |
| Armas | 6 | 16-20 | 10-14 nuevas + evoluciones |
| Power-ups | 8 | 16-20 | 8-12 pickups (XP gem, heart, magnet) |
| Efectos | 5 | 15-20 | 10-15 (hit flash, level up, damage numbers) |
| UI | 5 | 15-20 | 10-15 (XP bar, level up panel, minimapa, game over) |
| Fondos | 4 | 6-8 | 2-4 (boss arena especial) |

### Costo adicional estimado

| Categoría | Faltan | Costo |
|---|---|---|
| Zombis + bosses | 11 | $0.363 |
| Armas nuevas + evoluciones | 15 | $0.375 |
| Power-ups/pickups | 13 | $0.104 |
| Efectos | 15 | $0.150 |
| UI elements | 15 | $0.950 |
| Fondos + tilesets | 3 | $0.050 |
| **TOTAL adicional** | | **~$2.00** |

---

## Presupuesto Total de Sprites

| Concepto | Costo |
|---|---|
| Asset pack original | $1.37 |
| Sprites faltantes | $2.00 |
| **TOTAL COMPLETO** | **~$3.37** |
| Crédito restante (de $12) | $8.63 |

---

## PixelLab — Configuración para OpenCode

### MCP Server

```json
{
  "mcpServers": {
    "pixellab": {
      "url": "https://api.pixellab.ai/mcp",
      "transport": "http",
      "headers": {
        "Authorization": "Bearer TU_TOKEN"
      }
    }
  }
}
```

### Tools disponibles

- `create_character` — Personajes 4/8 direcciones
- `animate_character` — Animar personajes existentes
- `create_topdown_tileset` — Wang tiles top-down
- `create_sidescroller_tileset` — Tiles platformer
- `create_isometric_tile` — Tiles isométricos
- `create_map_object` — Objetos para mapas
- `generate-image` — Pixel art general
- `inpaint` — Editar sprites existentes
- `rotate` — Rotar sprites 4/8 direcciones

### Alternativa: itch.io packs

Complementar con packs de itch.io para ahorrar crédito:
- Objetos del mapa
- Efectos genéricos
- Tilesets básicos

---

## Referencia Visual

**Resolución recomendada:** 32x32 px (sweet spot del género)

**Paleta de colores:**
- Fondo: negro `#0a0a0a`, gris oscuro `#1a1a2e`
- Terreno: verdes apagados `#2d5016`, marrones `#3d2b1f`
- Zombis: verdes enfermos `#4a7c3f`, grises `#6b6b6b`
- Sangre: rojo oscuro `#8b0000`
- Player: amarillo `#ffd700` (contraste)
- Loot: dorado `#ffd700`, verde `#00ff41`
- UI: blanco con opacidad, rojo HP `#ff3333`, azul XP `#3366ff`

---

## Specs Técnicas

- **Framework:** SpriteKit (2D)
- **Persistencia:** UserDefaults o SwiftData
- **Audio:** AVFoundation
- **Haptics:** UIKit
- **Tamaño:** ~50MB
- **Plataforma:** iOS (iPhone + iPad)
- **Crédito PixelLab necesario:** $3.37 de $12

---

## Features de Adicción

- **"Una partida más"** — siempre mueres en un buen punto
- **Progresión visible** — cada muerte te hace más fuerte
- **Descubrimiento** — nuevas zonas, armas, zombis
- **Desafíos diarios** — "Mata 50 zombis sin morir" → 💎
- **Logros** — "Primer kill", "100 muertes", "Sin daño"
- **Boss fights** — cada zona tiene un boss final
- **Loot randomizado** — cofres con rarezas (común, raro, épico, legendario)

---

## Referencias del Género

| Juego | Personajes | Enemigos | Armas | UI |
|---|---|---|---|---|
| Vampire Survivors | 165 | 120+ | 80+ | HP, timer, level-up, chest |
| Brotato | 62 | 80+ | 80+ | HP, wave, shop, stats |
| HoloCure | 47 | 50+ | 30+ | HP, skills, weapon slots |
| 20 Minutes Till Dawn | 13 | 20-30 | 11 | HP, ammo, darkness |
| Soulstone Survivors | 23 | Bestiary extensa | 5×23 | HP, cooldowns, skill tree |
