---
title: MC Dashboard
date: 2026-09-16
tags: [project, minecraft, dashboard, monitoring, planeado, servidor]
status: completado
type: project
area: "[[Dev]]"
up: "[[Projects MOC]]"
related: ["[[Servidor Minecraft]]", "[[Projects MOC]]"]
---

# MC Dashboard

**Estado:** REDISEÑO COMPLETO desplegado (30 Sep 2026) — `http://100.107.196.60:8787` (solo Tailscale, con login). Reemplaza al deploy del 18 Sep (puerto 8090, retirado).
**Relacionado:** [[Servidor Minecraft]]

## Idea
Una **página web única** que muestre el estado del servidor Minecraft **en tiempo real** — lo mismo que el reporte semanal por correo, pero en vivo y sin esperar al domingo. Agrupa en una sola interfaz el monitoreo, el control del servidor y el estado de los backups, además de la salud del SSH de la laptop Ubuntu.

## Objetivo del usuario (verbatim)
> "vamos a crear una pagina wep organizada donde pueda ver en tiempo real el servidor osea REPORTE SEMANAL DEL SERVIDOR MINECRAFT todo eso pero de forma en viva tambien para agrupar tanto donde hacemos el backup como donde se controla el servidor para hacer una pagina wep donde pueda tener mas control de todo pero en una sola una interfza muy pulidad y 100% funcional tambien donde pueda ver el estado del ssh de la laptop donde esta ubunto"

## Requisitos identificados
1. **Monitoreo en vivo** — métricas estilo reporte semanal: TPS, MSPT, jugadores, RAM/CPU, temperaturas, mundo, backups
2. **Control del servidor** desde la misma interfaz — lo que hoy se hace en el panel Crafty (start/stop/restart, consola)
3. **Sección de backups** — estado local, Backblaze B2, copia en la laptop
4. **Estado del SSH / salud de la laptop Ubuntu**
5. **Una sola interfaz**, pulida y 100% funcional

## Datos técnicos de partida (ya existen)
- **API de Crafty v2:** `https://100.107.196.60:8443/api/v2` (login `admin`, pass solo en `/etc/mc-dashboard.conf` 600 root del servidor)
  - Comandos a la consola: `POST /servers/<SID>/stdin` con body **texto plano**
  - Stats: `GET /servers/<SID>/stats`
  - Acciones: `POST /servers/<SID>/action/start_server|stop_server|restart_server`
  - Backups: `POST /servers/<SID>/action/backup_server/<BACKUP_ID>` (ruta con el id; `POST` en `/backups/backup/<bid>` es RESTORE)
- **Server ID (SID):** `3c290914-d463-4e38-b896-3f1dbce26b5c`
- **Plantilla lógica:** `/usr/local/sbin/mc-status-report.sh` ya calcula todos los parámetros del reporte → es la base del dashboard
- **Canal seguro:** Tailscale (`100.107.196.60`) para acceder sin abrir puertos a internet

---

## Diseño aprobado (17 Sep 2026)

El usuario **aprobó el diseño** de la propuesta E. No se ha escrito ni una línea de implementación.

### Arquitectura
- **Backend:** Python de la **stdlib** (sin dependencias, sin npm, sin build), escuchando en el **puerto 8090** de la laptop Ubuntu (`mc-server`). Corre como servicio systemd.
- **Frontend:** **1 solo HTML** (CSS y JS embebidos) servido por ese mismo backend.
- **Acceso:** SOLO por **Tailscale** → `http://100.107.196.60:8090` (backend atado a esa IP con `HOST=100.107.196.60` en `/etc/mc-dashboard.conf`, 18 Sep 2026 — ni el WiFi local entra sin Tailscale; verificado: Tailscale 200, LAN rechazado).
- **Sin login** — la seguridad es la red (Tailscale + LAN), igual que el resto del setup.
- **Cero puertos abiertos a internet** — mismo principio que el servidor Minecraft.
- **Fuentes de datos:**
  - API de Crafty en `127.0.0.1:8443` (solo local, el backend hace de proxy)
  - `/usr/local/sbin/mc-status-report.sh` como plantilla lógica de qué leer y cómo
  - Logs del servidor (`latest.log`) y del sistema (`/var/log/*`, servicios, `systemctl`)

### 5 secciones (refresh cada 30 s)
1. **Principal (hero)** — ONLINE con glow, TPS gigante, facts, jugadores, acciones rápidas.
2. **Estado en vivo** — TPS, MSPT (sparkline), RAM, CPU, jugadores, versión, uptime.
3. **Consola / Control** — envío de comandos y botones de acción.
4. **Backups · 3 destinos** — Crafty local, Backblaze B2, copia en la laptop (nombre, tamaño, fecha, check).
5. **Equipo (laptop Ubuntu / Red)** — host, Tailscale, WiFi, temperaturas, batería, disco, servicios.
6. **Actividad (quest log)** — las tareas programadas como lista de misiones + actividad del log.

### Control seguro
- **Start / Stop / Restart** con **confirmación previa** (diálogo antes de ejecutar; el `restart` es el más delicado).
- **Comando** libre a la consola (equivalente a `POST /servers/<SID>/stdin`).
- **Backup manual** (dispara el `backup_server` configurado).
- **Kill items** (`kill @e[type=item]`) y **Save** (`save-all flush`) como acciones de una tecla.
- **SIN restore** — restaurar un backup desde el navegador es demasiado destructivo; se sigue haciendo a mano por SSH. Esta es una decisión de diseño explícita.

### Alertas
- **Banner rojo** en la cabecera cuando algo está mal (servidor caído, backup que no coincide, servicio parado, laptop sin cargador).
- **Correo a las 5:15** — el mismo `mc-backup-verify.sh` que ya existe sigue mandando la alerta/verificación diaria; el dashboard no lo reemplaza.

### Tema ganador
- **`propuesta-h-dock-secciones.html`** — evolución de la E con tema **8bitcn**: 6 tabs superiores con glow deslizante (el dock flotante se eliminó porque tapaba contenido), creeper-mascota con 3 caras por estado + banderita `n/12`, araña colgando del header, enderman patrullando el footer con partículas.
- **Descartadas:** A-NeonOnyx · B-Bloques · C-Admin · D-GraphiteMono · E-8bitcn (base de la H) · F-CreeperHUD · G-SerafimVerde.

### Mockups
Carpeta: `C:\Users\terrible019\Documents\mc-dashboard\mockups\` (**8 archivos**)

| Archivo                          | Tamaño   | Veredicto    |
| -------------------------------- | -------- | ------------ |
| `propuesta-a-neon-onyx.html`     | 21.429 B | Descartada   |
| `propuesta-b-minecraft.html`     | 21.182 B | Descartada   |
| `propuesta-c-admin.html`         | 17.322 B | Descartada   |
| `propuesta-d-graphite-mono.html` | 31.895 B | Descartada   |
| `propuesta-e-8bitcn.html`        | 36.470 B | Base de la H     |
| `propuesta-f-creeper-hud.html`   | 28.600 B | Descartada   |
| `propuesta-g-serafim-verde.html` | 38.193 B | Descartada   |
| `propuesta-h-dock-secciones.html`| ~81 KB   | **GANADORA** |

### Referencias 21st.dev (sesión 17 Sep 2026)
- Referencia de estilo hermana: [[Check Diario]] (tema Neon Onyx de serafimcloud; la propuesta A del dashboard lo reutilizaba y se descartó por repetitiva).
- Dock original pedido por el usuario: `21st.dev/@erikvalencia1/components/floating-dock-navigation` (implementado en la H, luego eliminado porque tapaba contenido y los iconos se veían negros).
- Navs evaluadas por investigador (recomendación: sliding glow): sidebar `unlumen/sidebar-001` · tabs `educalvolpz/animated-tabs` · glow `ruixen.ui/sliding-tabs` (elegida) · `codepen.io/rsmithrs/pen/RwxQjpZ` (magic menu, única vanilla) · bonus pixel `theorcdev 8bitcn` (encaje perfecto, animación mínima).
- Bonus: `21st.dev/@serafimcloud/themes/graphite-mono` (propuesta D, descartada: bonita pero no para este contexto).

### Proot de implementación
`C:\Users\terrible019\Documents\proot\proot-mc-dashboard.md` (4 fases: backend → frontend → QA → deploy).

---

## Pendiente
- [x] Brainstorming + spec de diseño (superpowers workflow) — **hecho 17 Sep 2026**
- [x] Definir stack (backend Python stdlib + 1 HTML, servicio systemd) — **hecho 17 Sep 2026**
- [x] Implementación — **hecho 18 Sep 2026** (proot en `proot-mc-dashboard.md`)

## Implementación (18 Sep 2026)

- **Producción:** `C:\Users\terrible019\Documents\mc-dashboard\` → `server.py` (506 líneas, stdlib) + `index.html` (~99 KB, port de la H) + `mc-dashboard.service` + `install.sh`, instalados en `/opt/mc-dashboard` con servicio systemd `mc-dashboard.service` activo.
- **QA:** shinobi-reviewer 3 pasadas — final UI 94 / Código 94 / API 93, APROBADO. Bugs cazados: `/` daba 404, login Crafty recursivo, `f0()` inexistente, CORS global, token 401 sin renovar, B2 sin ordenar, RAM en kB, temps/WiFi sin leer, regex de versión con mayúscula.
- **GOTCHAS:** Crafty escucha en `0.0.0.0:8443` (no solo localhost, preexistente — se deja porque el usuario entra al panel por Tailscale); rutas reales bajo `crafty-4/servers/<SID>/` (no `servers/<SID>/`); la IP LAN volvió a `10.0.0.134`.
- **Deploy:** scripts efímeros en `Temp/opencode` (`runts.py` por Tailscale, la IP vieja `.134` de `runsudo.py` está muerta); la pass de Crafty se extrajo **en el servidor** desde `mc-api-send.py` y nunca viajó a este PC.
- **Post-deploy (18 Sep, noche):** jugadores venían como string `"['terrible019']"` y se parsean con `ast.literal_eval`; MSPT ahora acumulativo en memoria (1 muestra/45 s hasta 60); verificado en vivo (TPS/RAM/Xms/CPU contra `ps`/log) y **visto bueno del usuario**.

## Rediseño completo (30 Sep 2026)

- **Frontend:** GPT-6 **Astra** en Codex (`designer-max`) — 6 tabs del original con su color, mundo voxel 3D, shaders, HUD glass, creeper + antorchas, microcopy en español. Login épico animado con `designer` (GPT-6.1 Sol). Proot Codex: `Documents\proot\proot-mc-dashboard-codex.md`.
- **Backend:** Python 3.12 stdlib aquí (`@data-ninja` + fixes directos) — `app/backend/` (server.py + mc/*.py), login con contraseña, SSE en vivo, proxy Crafty. Proot: `Documents\proot\proot-mc-dashboard.md`.
- **Deploy:** `/opt/mc-dashboard/app/{frontend,backend}/`, servicio systemd `mc-dashboard` (User=crafty), env `/etc/mc-dashboard.env` (password solo ahí), puerto **8787**, UFW solo tailscale0. Cero puertos públicos.
- **Terreno REAL verificado:** `/api/world/{meta,height.png,color.png}` generados desde los region files (grid 256, scale 8, ±1024). Parser NBT propio: TAG_Long_Array(12), heightmap post-1.16 (37 longs, 7×9-bit sin cruzar, +min_y −64), paleta 26.3 con `id`, blockstates padded por long. Primera pasada ~5 min, refresh incremental por mtime en segundos.
- **Contrato JSON congelado** (8 endpoints + SSE + 3 acciones) idéntico en ambos proots.

## Pase final (30 Sep 2026, noche)
- Temps NVMe/WiFi arregladas (hwmon `nvme` + `iwlwifi_1`), invite-card rediseñada, pase de belleza CSS-only del `designer` (mundo nocturno, verde unificado, `dashboard.css?v=3`).
- Producción: `http://100.107.196.60:8787` — verificado en vivo, temps reales cpu 40 / wifi 46 / nvme 34.
- Estrellas arregladas (`.viewport::after` a `z-index:1`, `dashboard.css?v=4`) + favicon FINAL: cara de creeper pixel art 16x16 (`assets/favicon-v2.svg`, verde `#4CAF50` sobre teja `#0c1011`, investigación en 20 fuentes).
- **Bug raíz de auth arreglado:** `mc/auth.py` usaba `base64.Error` (no existe) en el `except` → `AttributeError` en cada cookie inválida = causa de las caídas intermitentes del mundo. Ahora `binascii.Error`; cookies inválidas → 401 limpio. MSPT ya no sale duplicado.
- **Datos EN VIVO (SSE conectado, 30 Sep noche → 1 Oct):** `/api/events` empuja `status`+`players`+`logs` cada **3 s** y `world` cada **~60 s** (sello estable por mtime de regiones, sin churn). El backend muestrea el server cada **~8 s** (`_TTL`); la UI no re-renderiza si el payload no cambió (`liveSigs`). Sin recargar la página; reconexión automática. `app.js?v=8`, `world.js?v=4`.
- **MSPT reparado:** el log dejó de escribir "Server tick times"; ahora se usa `/tick query` → `Average time per tick: X ms` (real, ~7-20 ms). Historial del chart operativo.
- **Casa del mundo:** el mapa de terreno está centrado en el **Overworld X ≈ -478, Z ≈ -967** (superficie Y ≈ 67-69). Teletransportes con `execute in minecraft:overworld run tp @a -478 69 -967`.
- **Entidad "Alfred":** NO existe ninguna entidad guardada con ese nombre (barrido de 593 archivos de entidades / 20 857 chunks, 0 `CustomName` con "alfred"; `@e[name=Alfred]` no encuentra nada cargado).
- **Mapa in-game (estilo Xaero):** el usuario decidió **no** instalar nada. Xaero's es mod de **cliente**; las alternativas server-side (squaremap/BlueMap) quedaron descartadas.

## Enlaces
- Relacionado: [[Servidor Minecraft]]
- Hub: [[Projects MOC]] · [[Nexus]]
