---
title: "Tally — Design Spec"
date: 2026-06-23
type: spec
status: idea
project: Tally
tags: [tally, expense-tracker, design-spec]
up: "[[Projects MOC]]"
area: "[[Dev]]"
---

# Tally — Expense Tracker PWA

> Sistema de Presupuesto Conectado en Tiempo Real
> Diseño validado: 2026-06-23
> Pendiente implementación: Julio 2026
> **Proot:** [[10-Projects/Tally/2026-06-23-tally-proot-implementacion|Ver proot de implementación]]

## Stack
- HTML + CSS + JS vanilla (cero dependencias)
- IndexedDB (almacenamiento local)
- Notification API + Service Worker
- Cloudflare Pages ($0/mes, cuenta existente)
- Sin backend, sin servidor, sin costos recurrentes

## Arquitectura
```
tally/
├── index.html
├── manifest.json
├── sw.js
├── css/
│   └── style.css
└── js/
    ├── app.js          ← Entry point, routing, tabs, sheet, FAB
    ├── dashboard.js    ← Cuentas scroll horizontal, resumen, top categorías
    ├── lista.js        ← Todos los gastos filtrables
    ├── stats.js        ← Donut SVG, barras categoría + alertas, barras semanales
    ├── settings.js     ← Export/import, categorías, cuentas, reset
    ├── storage.js      ← IndexedDB wrapper
    └── notifications.js ← Notification API
```

## Tabs (4 + FAB)
| # | Tab | Contenido |
|---|-----|-----------|
| 1 | Dashboard | Scroll horizontal cuentas (débito/crédito/ahorros), gasto mensual total, top categorías con barras, botón "Pagar Tarjeta" en crédito |
| | FAB central | Sheet con toggle Gasto|Ingreso, cuenta preseleccionada (scroll horizontal arriba) + categoría + monto + fecha + nota opcional |
| 2 | Lista | Todos los gastos del mes, filtrables por categoría y cuenta |
| 3 | Stats | Donut SVG grande gasto total del mes → barras categoría con alertas 80%/100% → barras semanales (scroll) |
| 4 | Ajustes | Export/import JSON, categorías personalizables (predefinidas + crear nuevas), cuentas editables, reset datos |

## Ideas futuras (no implementar aún)

- **Integración Coda → Tally:** Importar JSON de Coda con total de suscripciones del mes y crear un gasto único en la cuenta que el usuario elija (ahorros o crédito). El botón iría en Ajustes como selector de archivos.
- **5to tab Suscripciones:** Opcional. Lista de suscripciones importadas de Coda con próximo cobro y monto. Icono de calendario/repeat.
- No requiere backend — mismo IndexedDB o import manual.

## Pendiente para implementación
- **Validación ingrediente contra OpenFoodFacts**: antes de guardar un gasto en IndexedDB, validar que el nombre del producto/ingrediente exista en la base de datos de OpenFoodFacts. Si no existe, mostrar advertencia o impedir guardar.
- **Fechas en labels**: las fechas de los label para poner o quitar dinero NO se implementaron en ningún lado. Pendiente decidir si los labels del sheet llevan fecha seleccionable o usan fecha actual por defecto.
- **Categoría "Regalo"**: en el menú de categorías del modo Gasto, NO incluir "Regalo" como opción. Un regalo no es un gasto.
- **Sin flechas en input monto**: el input `type="number"` del sheet debe ocultar los spinners nativos (up/down arrows) con CSS (`-webkit-appearance:none`, `-moz-appearance:textfield`).

## Reglas clave
- Gasto descuenta en tiempo real: la cuenta seleccionada baja, la barra de categoría sube
- Ingreso suma a la cuenta seleccionada
- Categorías predefinidas (8-10) + opción de crear nuevas escribiendo
- Presupuesto mensual opcional por categoría con alertas visuales al 80% (#B91C1C) y 100% (#7F1D1D)
- Múltiples cuentas (débito/crédito/ahorros), scroll horizontal en dashboard. Usuario crea/edita desde ajustes
- Cuenta de crédito: botón "Pagar Tarjeta" → descuenta de ahorros, resetea cupo
- Al añadir gasto/ingreso: toggle en sheet (Gasto|Ingreso), cuenta preseleccionada por defecto, se puede cambiar en scroll horizontal
- Reseteo mensual: presupuestos se resetean cada mes, saldos de cuentas NO

## Paleta
| Elemento         | Color                                                 |
| ---------------- | ----------------------------------------------------- |
| Fondo            | `#0A0A0A`                                             |
| Cards glass      | `rgba(22,22,26,0.7)` + borde `rgba(255,255,255,0.08)` |
| Acento principal | `#8B5CF6` (violeta amatista)                          |
| Versión pastel   | `#A78BFA` (hover/detalles)                            |
| Tarjeta crédito  | `#1E1E24`                                             |
| Texto primario   | `#FFFFFF`                                             |
| Texto secundario | `#9CA3AF`                                             |
| Alerta 80%       | `#B91C1C` (rojo vino)                                 |
| Alerta 100%      | `#7F1D1D` (rojo sangría)                              |

## Logo
- Fusión: barra horizontal T (blanca) + pila 3 monedas (violeta progresivo) como vertical
- Versión app icon 120×120px simplificada (sin tally marks)
- Sin tally marks en versiones pequeñas para claridad
