---
title: GTA 5 Mods
type: project
status: activo
created: 2026-08-28
up: "[[Projects MOC]]"
tags: [project, gta5, mods, gaming, setup]
---

# GTA V Legacy — Mods Instalados

## Juego
- **Versión:** Legacy 1.0.3889.0
- **Ubicación:** `C:\Program Files\Rockstar Games\Grand Theft Auto V Legacy`
- **Plataforma:** PC (Rockstar Games Launcher)

## Mods instalados (6)

### Base (3)
| # | Mod | Versión | Fuente |
|---|-----|---------|--------|
| 1 | Script Hook V | v3889.0 | dev-c.com |
| 2 | ScriptHookVDotNet Enhanced | v1.1.0.6 | gta5-mods.com |
| 3 | OpenIV | 4.1 Build 1502 | openiv.com |

### QOL (3)
| # | Mod | Versión | Notas |
|---|-----|---------|-------|
| 4 | VisualV | 1.0.630 | Gráficos vanilla-plus, 2-5% FPS impact. Instalado via .oiv en mods/ |
| 5 | Enhanced HUD V | 1.0.1 | Velocímetro. HP/Armor desactivados en EnhancedHUDV.ini |
| 6 | Matt's Authentic Euphoria | v1.1 | Animaciones de muerte realistas. Instalado via .oiv en mods/ |

## Config EnhancedHUDV.ini
- `EnableHUD=1`
- `[HP] Enable=0` (desactivado)
- `[Armor] Enable=0` (desactivado)
- `[Speedometer] Enable=1` (activo)

## Archivos importantes
- **Game root:** dinput8.dll, ScriptHookV.dll, ScriptHookVDotNet.asi/.dll, MinHook.x64.dll, EnhancedHUDV.asi/.ini
- **scripts/:** LemonUI.SHVDN3.dll + configs de Enhanced HUD V
- **mods/:** update.rpf (VisualV + Matt's Euphoria), update2.rpf (VisualV)

## Instalación OpenIV
- ASI Loader: INSTALLED
- OpenIV.ASI: INSTALLED
- openCamera: INSTALLED
- Carpeta mods/ activa

## Mods descartados
- Contract in SP — causaba bugs en prologue
- Odd Jobs SP — causaba bugs en prologue
- RealisticGunplayMod — causaba bugs (eliminado)

## Pendiente futuro

### Vehículos add-on
- Requiere Gameconfig + HeapAdjuster + Packfile Limit Adjuster
- Packs recomendados: Gta5KoRn (48 coches), Mega Addon Car Pack (115+), Realistic Car Pack (93)

### LSPDFR (15 mods)
Script Hook V + OpenIV (ya instalados), LSPDFR 0.4.9 (+ RAGE Plugin Hook), Stop The Ped, CompuLite CAD, Traffic Policer, Ultimate Backup, Police SmartRadio, ELS, Vehículos ELS, EUP, UnitedCallouts, More Robberies v1.8.0, Realistic Gunplay v2.0, Realistic Holster v3.6, Realistic Guns Sounds

## Reglas importantes
- NUNCA entrar a GTA Online con mods
- Script Hook V cierra el juego si detecta Online
- BattlEye puede bloquear dinput8.dll (desactivar desde launcher si es necesario)
