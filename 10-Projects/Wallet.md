---
title: Wallet
type: project
status: completado
prioridad: alta
inicio: 2026-09-20
created: 2026-09-20
updated: 2026-09-28
area: "[[Dev]]"
up: "[[Projects MOC]]"
related: ["[[Projects MOC]]", "[[Nexus]]"]
tags: [wallet, pwa, vanilla-js, indexeddb, productividad, obsidian-chalk]
---

# Wallet

App personal de control de gastos: PWA **offline first** para el día a día, sin servidor y sin cuentas. Todo vive en el dispositivo.

**Estado:** implementación completa — verificación estática OK. Pendiente prueba visual del usuario y deploy.

## Datos
- **Ubicación:** `C:\Users\terrible019\Documents\wallet\`
- **Stack:** HTML + CSS + JS vanilla, sin dependencias ni build. Persistencia con **IndexedDB**.
- **Diseño ganador:** v4 "Obsidian Chalk oscuro" — fondo `#06080b`, acento verde `#10b981`.
- **Navegación:** 5 tabs + FAB central.
- **Avatar reactivo:** 4 estados según el gasto del mes — 70% / 90% / 100%.
- **Multi-moneda:** RD$ y US$, con **tasa congelada** (se guarda la tasa usada en cada movimiento, no se recalcula).

## Contrato de datos
`docs\CONTRATO.md` — contrato congelado antes de implementar (esquema de IndexedDB + API interna).

## Modelo de agentes (por fases)
Proot: `C:\Users\terrible019\Documents\proot\proot-wallet.md` (fuera del vault). Fases con supervisor + agentes especializados y QA del shinobi-reviewer.

## Sesión 20 Sep 2026 — QA, correcciones y rediseño

**QA dio FAIL 58/100** con defectos críticos; se cerraron todos y se rehizo el rediseño de varias vistas.

### Bugs corregidos
- `js/store.js`
  - `updateTransaction` ahora acepta `amountMinor` (validado, recalcula el saldo y revalida la tasa FX).
  - Rechaza gastos sin categoría.
  - Nuevo helper `isLiveCategory()` — además de existir, la categoría no puede estar archivada (ni ser tombstone).
  - `exportCSV` paginado con cursor: ya no corta la exportación en 500 filas.
  - Presupuestos: se rechaza un límite ≤ 0 y los porcentajes ya no devuelven `Infinity` ni `NaN`.
  - `voidLinked` deja un tombstone atómico.
  - `importJSON` valida el esquema y la integridad referencial completa antes de escribir nada.
- `js/app.js`
  - Nuevos `toDisplayMinor()` / `sumDisplayMinor()`: convierten con la **tasa FX congelada**. Se usan en "gastado hoy", el pie de Movimientos, el total por día y el calendario (antes se sumaba RD$ y US$ en crudo).
  - Focus trap real en modales y sheet (`trapFocus` / `topFocusRoot` / `focusableIn` / `zIndexOf`), con wrap de Tab y restauración vía `focusSoft`.
  - Archivar una categoría padre con subcategorías o movimientos se bloquea con un toast.

### Rediseño
- **Vista Límites:** card global con encabezado punto + hairline + contador, filas tipo card con pastilla tintada por categoría, barra con degradado/glow/cascada y píldora de porcentaje con semáforo 70/90/100.
- **Modales** de recurrente, editar movimiento y límite con acento propio, card de campos y pie con botón primario.
- **Estados vacíos** unificados en un solo estilo.

### PWA
- `icons/icon-192.png` y `icons/icon-512.png` generados.
- `manifest.json` con `id` propio, iconos PNG y `maskable`.
- `sw.js` (cache v3) con los iconos en el precache.
- `apple-touch-icon` apuntando al PNG.

### Verificación
- Estática OK: `js/app.js`, `js/store.js` y `sw.js` pasan esprima; CSS con llaves balanceadas.
- Re-QA: **84/100**.

**Crash de arranque corregido (noche):**
- Error "Falta tasa congelada para conversión" tumbaba el Panel si había un movimiento en otra moneda que la base (cuenta US$ con base RD$, base US$ con datos RD$, o pata de transferencia entre monedas).
- `converted()` en store.js ya no lanza: 3 ramas (moneda, original exacto, fx a la otra moneda) y `null` si no hay tasa; los 4 reportes saltan los `null`.
- `toDisplayMinor()` en app.js alineado (la pata destino de transferencias se mostraba mal convertida).
- Overlay fatal: título y consejo de http ahora dinámicos (solo salen si falla IndexedDB de verdad); antes el título fijo "No se pudo abrir la base de datos" confundía.
- Caso real del usuario: base en US$ con datos en RD$ ("en 0 cuentas", 10 movimientos en US$ 0.00) → vuelve a RD$ en Ajustes. Pendiente opcional: tasa de conversión en Ajustes si quiere dólares.

## Pendiente
- [ ] Prueba visual del usuario en el navegador.

## Deploy (28 Sep 2026)
- **URL:** https://wallet-bwy.pages.dev
- Proyecto Cloudflare Pages `wallet` (subdominio `wallet-bwy`, el nombre `wallet` ya estaba tomado a nivel global).
- Script de subida: `wallet\tools\deploy.py` (upload directo por API, 9 assets). Credenciales solo por env `CF_API_TOKEN` / `CF_ACCOUNT_ID`.

## Plan final de cierre (mañana)

**3 visuales**
1. Modal de Moneda base como cards seleccionables + nota de consecuencia (aviso de qué pasa con los datos al cambiar la base).
2. Vista Movimientos al lenguaje nuevo (mismo estilo visual del rediseño).
3. Hero sin límite con CTA (cuando no hay presupuesto definido, el hero invita a crearlo).

**2 funciones**
1. Tasa USD↔DOP editable en Ajustes (conversión manual, sin recalcular movimientos ya guardados).
2. "Guardar y seguir" en el sheet (guardar el movimiento y dejar el sheet abierto para capturar otro seguido).

**Cara viva del avatar**
- Mismo aro, con parpadeo en idle, salto al ingresar, temblor cuando se acerca al límite, chip flotante `+RD$ X` y mini-confeti.
- Logo nuevo de Wallet basado en el avatar (favicon, iconos PWA y header).

**Cierre**
- Deploy a Cloudflare Pages y cierre del proyecto.
- Decisión final: el nombre SE QUEDA en "Wallet" (descartados Bolsillo, Alcancía, Fondo, Peso a Peso y Cartera).

## Sesión 23 Sep 2026 — avatar nuevo, fixes y QA final

**Avatar rehecho desde cero.** Una auditoría encontró **10 bugs**. Los peores: el topbar mostraba una letra W gris sin ningún estado de ánimo, el confeti se dibujaba por debajo del scrim del modal (se veía sin sentido) y había un parpadeo tipo glitch al cambiar de estado. Ahora es un **componente SVG único** reutilizado en el hero, el topbar y el rail, con estado **nervous** (cara con gota), timers unificados en un solo ciclo, y el **confeti eliminado** a petición del usuario.

**Bug grave de saldo corregido.** El pago de tarjeta **sumaba** dinero en lugar de restarlo. `balanceDelta()` ahora tiene **ramas explícitas** por tipo de movimiento (incluida `card_payment`) en vez de la heurística anterior.

**Efectivo coherente.**
- Las cuentas de efectivo se pueden **borrar o archivar**.
- Nunca puede quedar en **negativo**: validado en gastos, transferencias, pagos y recurrentes.
- El **seed es idempotente** (no duplica efectivo al re-sembrar).
- Los movimientos sobre cuentas **archivadas quedan bloqueados**.

**QA final: PASS.** shinobi-reviewer aprobó con **UI 94 / Código 96** — aprobada para publicación.

**Pendiente único:** deploy a Cloudflare Pages (falta el `CF_API_TOKEN` y el `CF_ACCOUNT_ID` del usuario).

## Enlaces
- Código: `C:\Users\terrible019\Documents\wallet\`
- Contrato: `C:\Users\terrible019\Documents\wallet\docs\CONTRATO.md`
- Proot: `C:\Users\terrible019\Documents\proot\proot-wallet.md`
- Hub: [[Projects MOC]] · [[Nexus]]
