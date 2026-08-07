---
title: "FitTrack"
type: project
status: idea
platform: iOS
framework: SwiftUI
up: "[[Nexus]]"
created: 2026-07-10
tags:
  - app
  - ios
  - fitness
  - healthkit
  - swiftui
---

# FitTrack — App de Ejercicios Personal

> App nativa iOS para trackear ejercicios, cardio y progreso fitness.

## Concepto

App de fitness personal que usa HealthKit para sincronizar datos del Apple Watch y iPhone. Tracking de ejercicios, rutinas, progreso y estadísticas.

## Features

### Core
- **Rutinas personalizadas:** crear rutinas con ejercicios y sets
- **Timer de descanso:** automático entre sets
- **Historial:** ver progreso por ejercicio (peso, reps, volumen)
- **Timer de sesión:** cuánto dura cada entrenamiento

### Cardio
- **GPS tracking:** ruta de correr/caminar con mapa
- **Métricas:** distancia, ritmo, elevation, calorías
- **Intervalos:** configurar sprints y descansos

### Estadísticas
- **Gráficas de progreso:** peso total, volumen semanal, PRs personales
- **Calendario:** días entrenados vs descanso
- **Resumen semanal/mensual

### Apple Watch
- **Complicación:** ver próximo ejercicio
- **WorkoutKit:** sesiones nativas en el Watch
- **Heart Rate:** frecuencia cardíaca en tiempo real

### Extras
- **Widgets:** "Hoy: Push day" o "Racha: 15 días"
- **Notificaciones:** recordatorio de entrenar
- **Dark mode:** diseño oscuro elegante
- **Export:** compartir progreso como imagen

## Stack

- **UI:** SwiftUI
- **Datos:** SwiftData
- **Salud:** HealthKit
- **Watch:** WatchKit + WorkoutKit
- **Maps:** MapKit (GPS cardio)
- **Charts:** Swift Charts (gráficas)
- **Widgets:** WidgetKit

## Specs Técnicas

- **Plataforma:** iPhone + Apple Watch
- **iOS:** 17+
- **watchOS:** 10+
- **Tamaño:** ~30MB
- **Offline:** sí (datos locales, sync con HealthKit)
