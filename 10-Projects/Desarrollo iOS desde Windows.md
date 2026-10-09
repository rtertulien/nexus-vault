---
title: Desarrollo iOS desde Windows — Ruta Gratuita
date: 2026-10-09
status: activo
type: project
tags:
  - project
  - ios
  - flutter
  - desarrollo
  - movil
up: "[[Mac Setup 2026]]"
---

# Desarrollo iOS desde Windows — Ruta Gratuita (sin Mac, sin $99)

Relacionado: [[Mac Setup 2026]] · [[iOS Projects]]

> Verificado 2026-10-09. Ruta para escribir apps Flutter en Windows, compilar iOS en la nube y instalarlas en el iPhone con Apple ID gratuito.

## Resumen
Windows no puede compilar iOS (Flutter exige macOS + Xcode). La ruta gratuita es: Flutter en Windows -> GitHub -> CI macOS en la nube (Codemagic) que genera el artefacto sin firmar -> Sideloadly lo firma con Apple ID gratuito -> SideStore en el iPhone renueva cada 7 días por Wi-Fi con LocalDevVPN.

## Herramientas instaladas (Windows)
- Flutter 3.47.7 stable (Dart 3.13.5) en `C:\src\flutter`; Git; Sideloadly; iTunes + iCloud; servicios Apple Mobile Device + Bonjour corriendo.

## Flujo por etapas
1. Desarrollar la app con Flutter en Windows (probar en emulador Android, web o desktop). Fijar un bundle id único (ej. `com.tuuser.miapp`).
2. Subir el repo a GitHub (`git push`).
3. Codemagic: vincular el repo, workflow en `mac_mini_m2`, build iOS sin firma (`flutter build ios --release --no-codesign` / `CODE_SIGNING_ALLOWED=NO`). Empaquetar `Payload/Runner.app` + comprimir a `.ipa` como artefacto.
4. Descargar el `.ipa` del build. Conectar el iPhone por USB. Sideloadly firma con el Apple ID gratuito e instala la app y SideStore.
5. En el iPhone: activar Modo Desarrollador, confiar en el perfil, activar LocalDevVPN, abrir SideStore e iniciar sesión. SideStore refresca las firmas cada 7 días por Wi-Fi.

## Límites de la ruta gratuita (Apple ID gratis)
| Límite | Valor |
|---|---|
| Apps simultáneas | 3 (SideStore cuenta como 1 -> 2 para tus apps) |
| Duración del certificado | 7 días (refresco semanal obligatorio) |
| App IDs nuevos | 10 por semana |
| Push remoto (APNs/FCM) | IMPOSIBLE (Apple niega el entitlement `aps-environment`) |
| Notificaciones locales | SÍ, máx ~64 programadas |
| Codemagic gratis | 500 min/mes de macOS (Mac mini M2) |
| GitHub Actions | minutos macOS gratis ilimitados SOLO en repos públicos |

## Puntos de fallo y solución
- Límite de 3 apps: dejar solo 2 apps + SideStore.
- Vencimiento a 7 días: abrir SideStore con LocalDevVPN en Wi-Fi al menos 1 vez/semana.
- "Developer Mode Required": Ajustes > Privacidad y seguridad > Modo de desarrollador (reiniciar).
- "Desarrollador no confiable": Ajustes > General > VPN y gestión de dispositivos > Confiar.
- Minutos de Codemagic: cachear CocoaPods/Flutter, compilar solo versiones de prueba listas.

## Correcciones importantes (verificado)
- La vía OFICIAL de SideStore usa **iloader** + iTunes/Apple Devices para la instalación inicial, NO Sideloadly. Sideloadly sirve como alternativa para firmar e instalar `.ipa`.
- El túnel oficial es **LocalDevVPN** (App Store); StosVPN es alternativa no oficial.
- iOS 26.4+ desactiva el refresco por datos móviles y algunos atajos: hace falta Wi-Fi real + VPN.
- Sideloadly y otras fuentes afirman "cada 4 días" de refresco; prevalece el dato de Apple (7 días).
- Codemagic no documenta un `.ipa` sin firmar listo: entrega un `.app`; hay que empaquetarlo en `Payload/` y comprimirlo a `.ipa`.

## Pendientes
- [ ] Confirmar la versión de iOS del iPhone (no se pudo leer por USB; iTunes no lo detectó).
- [ ] Confirmar en una build real de Codemagic si el artefacto sin firma se puede empaquetar a `.ipa` y lo acepta Sideloadly.
- [ ] Confirmar que Sideloadly o iloader instalan SideStore en esa versión de iOS.

## Fuentes
- Flutter docs (deployment/ios), docs.codemagic.io, docs.sidestore.io, developer.apple.com, docs.github.com, sideloadly.io. Verificación multi-fuente 2026-10-09.

---
Ver también: [[Mac Setup 2026]] · [[iOS Projects]]
