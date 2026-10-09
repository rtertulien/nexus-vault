---
title: Check Diario
type: project
status: completado
prioridad: media
inicio: 2026-09-17
created: 2026-09-17
updated: 2026-09-29
area: "[[Dev]]"
up: "[[Projects MOC]]"
related: ["[[Projects MOC]]", "[[Nexus]]"]
tags: [check-diario, vanilla-js, localstorage, productividad, neon-onyx]
---

# Check Diario

App web de escritorio (un solo dispositivo, sin servidor) para llevar el check diario de 6 cuentas donde el usuario ve videos/anuncios. El usuario es olvidadizo y quería saber "hoy me faltan dos cuentas" y revisar el historial como una historia.

**Estado:** funcional y aprobado (17 Sep 2026) + Fase 6 con vida + extras de dinamismo (animación del check, transiciones, fondo-respiración, banderín del muñeco, robot que habla solo). Deploy a Cloudflare Pages. Producción: https://check-diario.pages.dev. El usuario lo declaró "una de las mejores páginas que hemos hecho".

## Datos
- **Ubicación:** `C:\Users\terrible019\Documents\check-diario\`
- **Stack:** HTML5 + CSS3 + JS vanilla. CERO dependencias, cero build, cero npm. Se abre con doble clic desde `file://`. Único CDN: Google Fonts (Inter).
- **Tema:** Neon Onyx (21st.dev, autor serafimcloud). Tokens: fondo `#000000`, superficie `#0f0f0f`, superficie2 `#1b1b1d`, borde `#26272d`, texto `#ffffff` / `#bbcbb2` / `#838a7b` (este último subido desde `#6b7367` en QA para pasar WCAG AA), acento verde neón `#2fe92b`, parcial naranja `#ff9821`, peligro rojo `#ff5102`. Radios 6 y 9 px, esquinas casi rectas, tipografía Inter.
- **Layout:** "Panel lateral" — sidebar fijo a la izquierda (marca "6", nav Hoy/Historial/Cuentas, pie Exportar/Importar respaldo), contenido al centro, panel derecho con contador y estadísticas. Pensado para 1280–1920 px, NO móvil.

## Archivos
- `index.html` — 633 líneas
- `css/style.css` — 1014 líneas
- `css/views.css` — 408 líneas
- `css/critters.css` — 533 líneas (Fase 6: reloj de rodillos + mascotas)
- `js/store.js` — 314 líneas (17 métodos, con `setFx`)
- `js/app.js` — 140 líneas
- `js/critters.js` — 201 líneas (Fase 6: API `window.Critters` + frases del robot)
- `favicon.svg` — cara del muñeco pixel-art
- `tools/deploy.py` — despliegue a Cloudflare Pages sin Node
- `docs/CONTRATO.md` — contrato de datos + API congelada
- `mockups/` — 4 prototipos HTML del brainstorming

## Vistas
1. **Hoy** — anillo SVG que se vacía con el reloj en formato 12 h dentro y la etiqueta "para las 12:00 AM", chip "n/6 completadas", 6 filas con check, botón ojo que revela la clave 3 s, botón copiar, "Marcar las 6 del día".
2. **Historial** — calendario del mes: verde 6/6, naranja parcial, rojo 0/6, gris domingo libre, contorno verde en hoy; al tocar un día, el detalle con las cuentas que faltaron y las estadísticas.
3. **Cuentas** — agregar/editar/borrar, teléfono visible, clave oculta, interruptor activa/pausada, PIN opcional, exportar/importar respaldo JSON.

## Reglas de negocio
- El día va de 00:00 a 23:59 local y cambia solo (sin recargar) a medianoche; el día anterior queda solo lectura.
- El total del día se congela al primer check (`log` guarda `{checks, total}`).
- Domingos libres: no se exigen y no rompen la racha.
- Métricas: racha actual, mejor racha, % del mes y total completadas.
- Pausar una cuenta NO borra historial.
- PIN de 4-8 dígitos con SHA-256 + salt; "olvidé el PIN" lo reinicia sin borrar datos.
- El export lleva las claves en claro y la app avisa antes.

## Privacidad
Los números de teléfono y las claves NUNCA están en el código: se capturan por la UI y viven solo en `localStorage` del navegador (clave `check-diario.v1`). El HTML no trae ningún dato real, solo dummies.

## Modelo de agentes (7 agentes / fases)
Proot: `C:\Users\terrible019\Documents\proot\proot-check-diario.md` (fuera del vault).
- **Fase 0 — supervisor:** congeló `docs/CONTRATO.md` con la API de `window.Store` (16 métodos).
- **Fases 1-3 — designer:** HTML + los dos CSS. **data-ninja:** `store.js` + `app.js`.
- **Fase 4 — shinobi-reviewer:** QA en 2 etapas, 2 iteraciones.
- **Fase 5 — obsidian-curator:** documentación del vault.

## QA (shinobi-reviewer)
- **APROBADO en la iteración 3:** UI 94/100 · Código 92/100.
- Iteración 1 (UI 82 / Código 78, 14 defectos) e iteración 2 (UI 90 / Código 82, 1 regresión alta + 2 medios) rechazadas y corregidas.
- Bugs notables cazados: el rollover de medianoche devolvía al usuario a hoy cuando miraba un día pasado; pausar/reactivar una cuenta descuadraba el total (salía "6/5"); borrar la cuadrícula del calendario se llevaba por delante la fila L M M J V S D.

## Fase 6 — vida (17 Sep 2026, noche)
Contador con 6 rodillos `HH:MM:SS` que ruedan dígito a dígito (`--d` + `translateY`), dos puntos parpadeantes, anillo que drena el tiempo hasta medianoche y respira en warn/danger. Muñeco pixel-art patrullando el borde inferior (celebra al 6/6, pánico en rojo, duerme domingos) + robot HUD flotante cuyo ojo sigue el cursor y burbuja con el estado. Interruptor "Mascotas y animaciones" (`settings.fx`, `Store.setFx`). QA aprobado a la primera: UI 90 · Código 92 (2 LOW menores arreglados: verbo del anuncio y snap del rodillo 9→0). Proot: `C:\Users\terrible019\Documents\proot\proot-check-diario-fase6.md` (fuera del vault).

## Dinamismo extra (17 Sep 2026, noche, pedido: "quiero 4 5 2")
Sin agentes (cambios acotados, hechos directo): (1) transiciones entre vistas (`view-in`) + cascada del calendario al entrar a Historial o cambiar de mes (`is-cascade` + `--i`, solo cuadrícula grande); (2) fondo-respiración según la hora (`body::before` fijo + `body[data-ambience]` que `app.js` escribe cada segundo: ok/warn/danger/sunday); (3) muñeco con más vida: banderita `n/6` sobre la cabeza (`#pet-flag`, "libre" en domingo) + saltito al marcar (`is-dash` 900 ms vía `scan()`). Deploy `6266f333`, 8 archivos, verificado en vivo.

## Extras finales (misma noche)
- **Animación al marcar:** el check hace pop con rebote + anillo verde que se expande + gancho que se dibuja girando + flash en la fila. Solo `transform`/`opacity` (deploy `a6b70dc5`).
- **Banderín del muñeco (saga de 6 deploys):** empezó como texto `n/6` sobre la cabeza y se tapaba al saltar → se movió al costado como plaquita → se volvió banderín SVG cute que ondea (`flag-wave`) viviendo dentro de `.pet-body` → se corrigió el giro espejado (`svg:first-of-type`) → patrulla corta pegada a la izquierda → salto de celebración sincronizado → **fix definitivo:** animar el padre común `.pet-body` (el cuerpo SVG tiene escala 2.5x y volaba el doble de alto que la bandera; confirmado en foros que cada SVG anima en sus coordenadas locales). Deploys `9d2ece0f`, `0ecfd626`, `fb6ab9b1`, `fd209cad`, `23b8eb67`, `78d1beb5`.
- **Robot que habla solo:** la burbuja rota frases cada 7 s según el estado (en peligro apura, parcial anima, 6/6 orgulloso, domingo zen). Deploy `02bc8e2b`.
- **Favicon:** cabeza del muñeco recortada de su SVG (baldosa `#0f0f0f`, 989 B). Deploy `09a2a62e`.
- **Modelo `shinobi-reviewer`:** deepseek-v4-pro → gpt-5.6-luna (el Pro disparaba la cuota; hay que reiniciar opencode para aplicar).

## Deploy
Sin Node en el equipo: `tools/deploy.py` reimplementa el Direct Upload de wrangler (token por variable de entorno `CF_API_TOKEN`, nunca en archivos; hash BLAKE3 de base64+extensión; 4 etapas: upload-token → check-missing → upload/upsert → deployment con manifest). Producción: **https://check-diario.pages.dev**. Favicon: cara del muñeco en `favicon.svg`.

## Pendiente
- [ ] Pulsar **«Contar el día de hoy»** en las 15 cuentas reales y comprobar mañana que el acumulado sube solo (**+$10,195 por día laborable**).
- [ ] Menor: `removeAccount` no toca el `log` (borrar una cuenta activa hoy deja congelado el total de ese día; es conforme al contrato, solo asimetría de UX).

## Sesión 28 Sep 2026 — festivos, orden y dinero

### Días sin trabajo
`settings.holidays` registra festivos y vacaciones. Un día libre se comporta como domingo: no se puede marcar, no rompe la racha y queda fuera del porcentaje mensual. API de `Store`: `isHoliday`, `toggleHoliday`, `addHolidayRange(from,to)`, `removeHoliday` y `nextWorkday`. En la UI se puede marcar un día desde el detalle del Historial y gestionar rangos en **Cuentas → Días sin trabajo**. El resumen anuncia el día exacto de vuelta (por ejemplo, «vuelves el lunes 28») y el contador queda inactivo en días libres.

### Reordenar cuentas
Nuevo `js/sortable.js`, sin dependencias: usa Pointer Events, mide los rects una vez por gesto y anima solo `transform`. Incluye handle visible, menú «Mover» (Al principio/Subir/Bajar/Al final), teclado (Enter/Espacio, flechas y Escape) y anuncios `aria-live`; no depende exclusivamente del arrastre y contempla WCAG 2.5.7. `Store.reorder(orderedIds)` persiste el orden y `mergedOrder` conserva la ranura de las cuentas pausadas. Referencias visuales: componentes `ai2/item-drag` y `rmahammad/kanban-card-movement` de 21st.dev.

### Ingresos y exportación Excel
Cada cuenta tiene `income` (ingreso fijo diario de lunes a sábado), `balance` (dinero base) y `balanceDate` (fecha «Desde»). Domingos y festivos no generan; `total = balance + generated`. El campo **Desde** ancla el inicio y permite recuperar el histórico. El Excel ahora es un `.xlsx` real generado sin librerías en `js/xlsx.js` (CRC32, ZIP y OOXML): teléfono como texto, acentos UTF-8, encabezado congelado, formato `"RD$ "#,##0.00` y fila TOTAL. `Store.countToday()` ancla hoy como un día contado sin alterar el acumulado; resta el ingreso de la base y fija la fecha base en ayer. Es idempotente.

### Demo, QA y despliegue
`demo.html` está publicada en https://check-diario.pages.dev/demo. Usa el motor real con localStorage en memoria, reloj simulado, tres cuentas de ejemplo y controles para avanzar día/semana/lunes, marcar festivos y reiniciar; permite comprobar el incremento automático y que los días libres no generan. Las fases pasaron QA del `shinobi-reviewer` con rúbrica 0–100. Despliegues en Cloudflare Pages; el token de API vive solo en variables de entorno/memoria, nunca en archivos.

## Sesión 28–29 Sep 2026 — cuentas de 7 días y retiros

### Cuentas que trabajan siete días
Las cuentas tipo Diamante/Rubí pueden activar el check **«Trabaja domingos y feriados»** (`workWeekends`). Esas cuentas generan ingresos también domingos y festivos; las demás mantienen el cálculo de lunes a sábado.

### Retiros
Se añadió la pestaña **Retiros**. Cada cuenta puede tener un único día semanal de retiro (por ejemplo, miércoles, jueves o viernes) y muestra su estado: **Disponible**, **No es su día**, **Faltan $X** o **Retirado hoy**. El botón **Retirar** descuenta el monto del acumulado, reinicia el conteo desde hoy y registra la operación en el historial. Solo se permite un retiro por cuenta y día.

Los montos son automáticos: se asigna el mayor umbral alcanzado por el acumulado entre **RD$5,500 / 20,000 / 80,000 / 200,000 / 800,000**; no se selecciona manualmente el rango.

### Correcciones y exportación
- El Excel (`.xlsx`) usa el orden: **Cuenta · Teléfono · Ingreso diario · Dinero acumulado · Total generado · Disponible para retiro**.
- Se corrigió el doble conteo de hoy al editar el monto base sin cambiar «Desde»: el conteo se ancla a hoy. También se añadió **«Quitar el día de hoy»** para deshacer un día contado de más.
- El pie del menú lateral muestra la versión para confirmar que está cargada la subida más reciente.
- La URL estable de producción es **https://check-diario.pages.dev** y se actualiza con cada subida; las URL con hash son alias de verificación de deployments, no cambian la dirección habitual.
- El usuario marcó checks por error alrededor de medianoche; lo resolvió restaurando una copia de respaldo propia.

## Enlaces
- Código: `C:\Users\terrible019\Documents\proyectos-opencode\check-diario\`
- Proot: `C:\Users\terrible019\Documents\proot\proot-check-diario.md`
- Hub: [[Projects MOC]] · [[Nexus]]
