---
type: project
status: activo
prioridad: alta
inicio: 2026-09-16
area: "[[Dev]]"
up: "[[Projects MOC]]"
related: ["[[Projects MOC]]", "[[Homelab - Mini PCs]]"]
tags: [project, minecraft, servidor, tailscale, playit, homelab, chunky, backups, cron, b2, rclone, retencion, alarma, restauracion, monitoring, granja-enderman, spawn, falso-positivo]
stack: [paper, crafty, tailscale, playit, ubuntu-server]
---

# Servidor Minecraft

**Estado:** Activo (16 Sep 2026)
**Stack:** Paper 26.3 + Crafty Controller 4.10.8 + Tailscale + playit.gg

## Resumen
Servidor de Minecraft para jugar con amigos, montado en una laptop vieja con Ubuntu Server. Cero puertos abiertos a internet: todo el acceso es por VPN Tailscale (el dueño) + túnel playit.gg (los amigos).

## Hardware
- Laptop vieja: Intel i3-1115G4 (2 núcleos / 4 hilos), 7 GB RAM, 232 GB NVMe
- Ubuntu Server 26.04.1 LTS "resolute", hostname `mc-server`
- IP LAN 10.0.0.134 (red `Tertulien 5G`, desde 7-oct-2026) · IP Tailscale 100.107.196.60

## Software
- **Crafty Controller 4.10.8** — panel web con interfaz gráfica (systemd service `crafty`)
  - URL: https://192.168.39.109:8443 (LAN) o https://100.107.196.60:8443 (Tailscale)
  - Usuario `admin` / contraseña `colombia2811`
- **Paper 26.3** — última versión de Minecraft (el versionado pasó a 26.x por año)
  - Server ID `3c290914-d463-4e38-b896-3f1dbce26b5c`
  - Puerto 25565, 12 jugadores, RAM 2-4 GB con flags Aikar
  - Auto-arranque activado (auto_start=1, delay 20s, crash_detection=1)
  - Modo no-premium (`online-mode=false`), `view-distance=14`, `simulation-distance=6`

## Seguridad (lo más importante)
- **CERO puertos abiertos a internet.** UFW con default deny incoming; solo 22/tcp (SSH) y todo el tráfico entrante por la interfaz `tailscale0`.
- Tailscale: server `minecraft-server` = 100.107.196.60; PC Windows `terrible19` = 100.96.216.96.
- Los amigos NO tocan la red: entran por un túnel de **playit.gg** (agente 1.0.10, systemd `playit.service`, agent id 02c7fcc8-f5be-4e37-9ab0-37afea4ccdcc).
- **Modo no-premium:** con `online-mode=false` cualquiera que tenga la dirección de playit puede entrar. Pendiente como opción: activar whitelist.

## Optimizaciones aplicadas
- Flags Aikar completas de JVM
- `view-distance=14`, `simulation-distance=6`
- Autosave 6000 ticks, `max-auto-save-chunks-per-tick: 8`
- `optimize-explosions: true`, redstone `ALTERNATE_CURRENT`
- Hoppers optimizados, `disable-world-ticking-when-empty: true`, armor stands sin tick
- `merge-radius` item 2.5 / exp 3.0
- `entity-activation-range` monsters 24, `entity-tracking-range` players 64 (menos ancho de banda por el túnel)
- `max-tnt-per-tick: 50`
- `spawn-limits` monsters 50 / animals 8, `monster-spawns` cada 4 ticks
- `chunk-system` io-threads y worker-threads fijados a 2 (por los 2 núcleos)
- Plugin **Chunky 1.5.3**

## Pre-generación del mundo
- Chunky con radio 2000 bloques (círculo) = **63,001 chunks al 100% en 19 min 21 s** a ~50 chunks/segundo
- El mundo pasó de 17 MB a 583 MB (100 region files)
- Estructura del mundo en MC 26.3: `world/dimensions/minecraft/<overworld|the_nether|the_end>/region/*.mca` (ya NO es `world/region/`)
- Ese mundo pre-generado (583 MB) se respaldó y luego se borró al migrar los mundos del usuario

## Pre-generado total con Chunky (16 Sep 2026, noche)
- **Overworld:** radio 3000, forma círculo, centro 0 0 → **142,129 chunks en 18 min 51 s**
- **The Nether:** radio 1500, círculo, centro 0 0 → **35,721 chunks en 2 min 20 s**
- **The End:** NO se pre-generó (ya tenía 86,986 chunks y está vacío)
- El mundo pasó de **1.7 GB a 2.3 GB**; region files: overworld **365**, the_nether **67**, the_end **241**
- Comandos usados: `chunky world <mundo>`, `chunky shape circle`, `chunky center 0 0`, `chunky radius <N>`, `chunky start`, `chunky progress`

## Zona horaria + tareas programadas (schedules)
- Zona horaria del servidor cambiada de `Etc/UTC` a **America/Santo_Domingo (AST, -0400)**
- **Importante:** Crafty lee la zona horaria solo al arrancar → hay que reiniciar el servicio `crafty` después de cambiarla para que el scheduler recalcule los cron
- 5 tareas creadas en Crafty vía API `POST /api/v2/servers/<SID>/tasks` (Server ID `3c290914-d463-4e38-b896-3f1dbce26b5c`):

| Hora | Frecuencia | Cron | Acción |
|---|---|---|---|
| 4:55 AM | cada 2 días | `55 4 */2 * *` | Aviso `say [SERVIDOR] Reinicio programado en 5 minutos...` |
| 5:00 AM | cada 2 días | `0 5 */2 * *` | `kill @e[type=item]` (limpia items del suelo) |
| 5:01 AM | cada 2 días | `1 5 */2 * *` | `save-all flush` |
| 5:02 AM | cada 2 días | `2 5 */2 * *` | Reinicio del servidor (`action`/`command` = `restart_server`) |
| 4:00 AM | diario | `0 4 * * *` | Backup automático (`backup_server`) |

- **Detalle técnico clave:** el dispatcher de Crafty usa la columna `command` (no `action`); para reiniciar hay que poner `action="restart_server"` **y** `command="restart_server"`
- Un reinicio **NO** borra los items del suelo por sí solo; por eso se añadió la tarea de `kill @e[type=item]` a las 5:00
- Verificado en vivo: el reinicio por API funcionó (PID del proceso java cambió 16145 → 17145 en ~10 s)

## Backups automáticos + copia a la laptop
- Configuración de backup en Crafty: backup_id `b551c248-0f35-4655-af84-2fe68e57c9fb`, nombre `auto`, ruta `/var/opt/minecraft/crafty/backups/3c290914-d463-4e38-b896-3f1dbce26b5c`, comprimido (zip), con parada del servidor antes de copiar (`shutdown=true`), conserva **solo el backup más reciente (1)** en el servidor (antes 3; cambiado por petición del usuario el 16 Sep 2026)
- **Editar la retención por API:** `PATCH /api/v2/servers/<SID>/backups/backup/<BID>` con body `{"max_backups":1}`. **Ojo con la ruta:** lleva el segmento extra `/backup/` (`/backups/backup/<bid>`); sin él devuelve 404. Rutas reales de backups: `GET|POST /api/v2/servers/<sid>/backups/`, `GET|PATCH|DELETE /api/v2/servers/<sid>/backups/backup/<bid>`, `DELETE /api/v2/servers/<sid>/backups/backup/<bid>/files` (body `{"filename":"x.zip"}`), `GET .../backup/<bid>/download/<file>`. **Ojo:** `POST /backups/backup/<bid>` = RESTORE (¡cuidado!). El prune de Crafty está en `app/classes/shared/backup_mgr.py:430`: borra si `len(list_backups) > max_backups` y `max_backups > 0`
- **Ubicación real del zip:** `backup_location/<backup_id>/<fecha>.zip` (Crafty añade una subcarpeta con el id del backup). Ejemplo real: `2026-09-16_18-53-58.zip`, **1.72 GB**, 2,190 entradas (mundo + configs). Ciclo completo stop → zip → arranque ≈ 1 minuto
- Lanzar un backup manual: `POST /api/v2/servers/<SID>/action/backup_server/<BACKUP_ID>`
- **Copia a la laptop principal (Windows):**
  - Clave SSH ed25519 sin contraseña `C:\Users\terrible019\.ssh\mc_backup` instalada en el `authorized_keys` del usuario `terrible`
  - Script `C:\Users\terrible019\MC-Backups\pull-backups.ps1`: busca los .zip nuevos por SSH/SCP, los descarga y conserva **solo el backup más reciente (1)** localmente (`$Keep = 1`, antes 5; log en `pull.log`)
  - Tarea programada de Windows `MC-Backups-pull` todos los días a las **5:30 AM** (modo "solo interactivo": requiere laptop encendida y usuario con sesión iniciada)
  - Prueba real: descargó **1,644.7 MB en 127 s** (~13.6 MB/s) a `C:\Users\terrible019\MC-Backups\`; zip validado (2,190 entradas)
- **Flujo diario resultante:** 4:00 AM backup en el servidor → 4:30 AM subida a la nube → 5:15 AM verificación con alarma → 5:30 AM descarga a la laptop. En días alternos además: 4:55 aviso, 5:00 limpieza de items, 5:01 guardado, 5:02 reinicio. **Domingos 6:00 AM: reporte semanal por correo** (ver sección "Reporte semanal por correo")
- Disco C: de la laptop: 691.8 GB libres

## Backup offsite en la nube (Backblaze B2)
- **Objetivo:** que los backups no dependan de que la laptop Windows esté encendida a las 5:30 AM. El servidor sube los backups a la nube por sí solo.
- **Se eligió Backblaze B2 en vez de Google Drive.** Motivos (verificados): (1) rclone retira durante 2026 el client_id compartido que traía por defecto, así que en Google Drive ahora es obligatorio crear un OAuth client_id propio en Google Cloud Console; (2) las Service Account de Google NO sirven en cuentas personales de Gmail (error "Service Accounts do not have storage quota", solo funcionan en Shared Drives de Google Workspace); (3) Google Drive gratis son 15 GB compartidos entre Gmail, Drive y Photos, y con backups de 1,6 GB se llena en ~9 días. B2 en cambio se configura con una sola clave API, sin OAuth ni navegador.
- **Instalado:** `rclone v1.75.1` en el servidor (instalado con el script oficial `curl https://rclone.org/install.sh | bash`; el paquete de los repositorios de Ubuntu 26.04 es la versión vieja 1.60.1).
- **Bucket:** `mc-backups-rt2811` (privado, plan gratis de 10 GB). Credenciales guardadas en el servidor en `/root/.config/rclone/rclone.conf` (permisos 600, solo root). Configuración con `hard_delete=true` (importante: sin eso Backblaze solo "oculta" los archivos borrados y siguen ocupando espacio).
- **Script de subida:** `/usr/local/sbin/mc-backup-upload.sh` (root). Busca los .zip de backup (Crafty los guarda en subcarpetas dentro de la carpeta de backups, por eso hay que buscarlos recursivamente), los sube a la carpeta `backups` del bucket y luego **conserva solo el backup más reciente (1)** (`KEEP = 1`, antes 5), borrando los viejos. Deja registro en `/var/log/mc-backup-upload.log`. Mecanismo de rotación: `rclone lsf | sort -r` y `rclone deletefile` de todo lo que pase de KEEP (con `hard_delete=true` libera cuota de verdad).
- **Programación:** timer de systemd `mc-backup-upload.timer`, todos los días a las **4:30 AM** (hora de República Dominicana). Tiene activado "Persistent=true", así que si el servidor estuviera apagado a esa hora, ejecuta la subida en cuanto vuelva.
- **Prueba real:** subió el backup de 1,61 GB en **129 segundos** (~13,4 MB/s). Integridad verificada: el SHA1 local y el de la nube coinciden (`8d53c90717aee94fc2d5120e156a1a44701b8669`). Nota técnica: en B2 hay que comparar con `rclone hashsum sha1`, porque `rclone md5sum` no funciona (B2 usa SHA1 para archivos grandes).
- **Flujo de backup completo (todo automático):**
  1. 4:00 AM — Crafty crea el zip del mundo (guarda solo el último en el servidor)
  2. 4:30 AM — subida automática a Backblaze B2 (guarda solo el último en la nube)
  3. 5:15 AM — verificación del backup + alarma por correo si algo falla (ver sección "Alarma por correo")
  4. 5:30 AM — si la laptop está encendida, descarga una copia local a `C:\Users\terrible019\MC-Backups` (conserva solo el último)
  5. **Domingos 6:00 AM** — reporte semanal por correo con el estado completo (ver sección "Reporte semanal por correo")
- **Retención (16 Sep 2026, noche):** por petición del usuario ("no quiero la vieja, quiero siempre las versiones actuales"), los **tres niveles conservan SOLO el backup más reciente**: Crafty `max_backups=1`, script B2 `KEEP=1`, script laptop `$Keep=1`. Verificado en vivo: se subió un zip de prueba viejo (1 KB) al bucket y al ejecutar el script de subida la rotación lo borró automáticamente, quedando un solo objeto (1,606 GiB).
- **Riesgo anotado:** con una sola copia, si el mundo se corrompe y luego corre un backup automático, la copia buena se pierde. Se le ofreció al usuario volver a conservar 2.
- **Espacio en B2:** con retención de 1, el bucket ocupa solo ~1,6 GB de los 10 GB gratis → el problema de margen de espacio queda resuelto. Se recomendó igualmente configurar un tope de gasto (Caps & Alerts) en Backblaze para no recibir cargos por sorpresa.

## Prueba de restauración (16 Sep 2026, noche)
- Se descomprimió el backup real `2026-09-16_18-53-58.zip` (1,72 GB) en una carpeta temporal (`/var/tmp/restore-test`) y se comparó con el mundo vivo del servidor.
- **Resultado: PASÓ.** `unzip -t` verificó los **2,190 archivos con 0 errores** ("No errors detected in compressed data").
- Las **3 dimensiones** presentes; mismos region files: overworld **365**, the_nether **67**, the_end **241**.
- Mismo conteo de chunks en backup y en vivo: overworld **197,020** + the_nether **40,183** + the_end **86,986** = **324,189**.
- **Prueba criptográfica:** el sha256 del archivo de región `r.-1.-1.mca` es idéntico en el backup y en el mundo vivo (`90a14e08004fcc6e57232b25a9b13c08`).
- `spigot.yml`, `bukkit.yml`, `ops.json` y `whitelist.json` con el mismo sha256. `server.properties` y `level.dat` SÍ difieren, pero es esperado y benigno: Paper reescribe `server.properties` al arrancar y `level.dat` cambia en cada guardado.
- **Conclusión: el backup es fiel y restaurable.**

## Alarma por correo (16 Sep 2026, noche)
- **Objetivo:** avisar por correo a `rtertulien019@gmail.com` si algo falla en el backup (petición explícita del usuario).
- **Montaje:** `msmtp` (config en `/etc/msmtprc`, permisos 600 root) usando SMTP de Gmail (`smtp.gmail.com:587` con TLS) y una **contraseña de aplicación** de Google de 16 caracteres. La contraseña NO se documenta aquí: vive solo en `/etc/msmtprc` (600 root).
- **Requisito para crear contraseñas de aplicación:** tener activada la **verificación en dos pasos** en la cuenta de Google. Si no está activada, la página de contraseñas de aplicación muestra "La opción de configuración que buscas no está disponible para tu cuenta".
- **Script de verificación:** `/usr/local/sbin/mc-backup-verify.sh`, corre todos los días a las **5:15 AM** (timer de systemd `mc-backup-verify.timer`, con `Persistent=true`). Revisa:
  1. Que el servidor Minecraft esté corriendo (`pgrep paper.jar`)
  2. Que el backup más reciente exista, tenga **menos de 26 horas** y pese **más de 500 MB**
  3. La integridad real del zip con `unzip -t`
  4. Que el backup haya llegado a la nube (Backblaze B2) con el mismo nombre y tamaño, y que no haya **más de 3 objetos** (rotación rota)
  5. Más de 20 GB de disco libre
  6. Que `crafty`, `playit` y `tailscaled` estén activos
  7. Que la laptop esté enchufada (ADP0 online)
- **Comportamiento:** si algo falla → correo de **ALERTA**. Si todo está bien → silencio. El latido dominical se desactivó (16 Sep 2026) para no duplicar correos; ahora esa función la cumple el **reporte semanal** (ver sección "Reporte semanal por correo").
- **Parche al script de subida:** `/usr/local/sbin/mc-backup-upload.sh` ahora avisa por correo si la subida a Backblaze falla.
- **Script auxiliar:** `/usr/local/sbin/mc-notify.sh` (envía el correo; uso: `mc-notify.sh "ASUNTO" "CUERPO"`).
- **Logs:** `/var/log/mc-backup-verify.log`, `/var/log/mc-notify.log`, `/var/log/msmtp.log`.
- **Prueba real:** se envió un correo de prueba y Google lo aceptó (exitoso).
- **Limitación conocida:** la verificación no comprueba el canal de correo en sí; si la contraseña de aplicación caduca, se detecta porque dejaría de llegar el reporte semanal de los domingos.

## Falsa alarma del verificador (17 Sep 2026)
- **Síntoma:** a las 05:15 AST la verificación automática envió el correo *"La nube NO tiene el ultimo backup o el tamaño no coincide — En la nube: 2026-09-17_04-00-07.zip (0 bytes) / Local: (1728393336 bytes)"*.
- **No había ningún problema real:** el zip se creó a las 04:00 (1.728.393.336 bytes, 2,195 archivos, íntegro) y la subida de las 04:30 terminó en 90 s; `rclone` confirmó 1 objeto de 1.610 GiB con el mismo tamaño exacto.
- **Causa raíz:** bug de parseo en `/usr/local/sbin/mc-backup-verify.sh` (línea 69): `rclone lsf --format sp` devuelve `TAMAÑO;NOMBRE` (el nombre va DESPUÉS del `;`), pero el script buscaba `"nombre;"` → la variable del tamaño quedaba siempre vacía. El bug estaba latente porque la verificación nunca había corrido por timer hasta ese día.
- **Fix aplicado** (respaldos en `/tmp/opt/mc-backup-verify.sh.bak` y `.bak2`): `RSIZE=$(rclone lsf "$REMOTE" --files-only --format sp 2>/dev/null | awk -F';' -v n="$NAME" '$2==n{print $1; exit}')`. Verificado: `OK nube=2026-09-17_04-00-07.zip size=1728393336` y `todo OK (sin alerta)`.
- **Mejoras añadidas al mismo script:** las comprobaciones de servicios (`crafty`/`playit`/`tailscaled`) y de batería ahora también anotan su estado en el reporte cuando todo va bien (antes solo logueaban en fallo); y el caso "laptop sin cargador" ahora sí dispara la alerta (`ALERTA=1`, antes solo añadía texto a PROBLEMAS sin enviar correo).
- **Correo correctivo enviado:** `[SERVIDOR MC] Falsa alarma corregida - el backup estaba bien` (11:35 AST, SMTP 250 OK).
- **Lección:** el primer día que corre un script nuevo hay que comprobar si un "FALLO" es real o es un bug del propio script. Y `rclone md5sum` no sirve en Backblaze B2 (usa SHA1): comparar con `rclone hashsum sha1`.

## Reporte semanal por correo (16 Sep 2026, noche)
- **Qué es:** cada **domingo a las 6:00 AM AST** el servidor envía a `rtertulien019@gmail.com` un correo con el estado completo del servidor (asunto `[SERVIDOR MC] Reporte semanal - DD/MM/AAAA`). Se eligió domingo porque va después de toda la cadena de backups de la madrugada.
- **Qué incluye el reporte:**
  - **Servidor Minecraft:** estado y PID, tiempo encendido, RAM del proceso, CPU (de un núcleo), versión de Paper, jugadores, TPS, MSPT, último arranque
  - **Laptop del servidor:** uptime, carga (load 1/5/15), RAM, swap, disco, frecuencia media, governor, temperaturas CPU/WiFi/NVMe, eventos de throttling de la semana (delta), energía
  - **Mundo:** tamaño y regiones por dimensión
  - **Backups:** estado local, nube (Backblaze B2), última revisión, última subida
  - **Servicios y red:** crafty, playit, tailscaled, ssh; estado de playit (Phase); WiFi (SSID, señal, rx/tx); IP LAN y Tailscale; URL del panel
  - **Actividad de la semana:** entradas y muertes de jugadores
  - **Errores en el log:** actual vs historial
  - **Tareas automáticas:** resumen de todos los schedules
- **Componentes:**
  - `/usr/local/sbin/mc-status-report.sh` — script que genera el reporte y lo envía con `mc-notify.sh`. Log en `/var/log/mc-status-report.log`, salida temporal en `/tmp/mc-status-report.txt`
  - `/usr/local/sbin/mc-api-send.py` — helper que envía UN comando a la consola del servidor vía la API de Crafty (login → `POST /servers/<SID>/stdin` con body texto plano). **OJO: lleva la contraseña embebida en texto plano; no copiar su contenido a ningún sitio de memoria**
  - `/etc/systemd/system/mc-status-report.timer` — `OnCalendar=Sun *-*-* 06:00:00`, `Persistent=true`, `AccuracySec=1min`, enabled + active
- **Detalle:** se **desactivó el latido dominical** de la alarma diaria (`mc-backup-verify.sh`; el check `date +%u = 7` se cambió a un valor que nunca se cumple) para no enviar dos correos el mismo día. Ahora el reporte semanal cumple también la función de comprobar que el canal de correo sigue vivo: si un domingo no llega, algo anda mal.
- **GOTCHAS resueltos:** (1) el MSPT va en la línea siguiente al mensaje "tick times" → `awk '/tick times/{getline l} END{print l}'`; (2) el contador de throttling acumulado de por vida parecía alarmante → se reporta el delta semanal guardando el valor anterior en `/var/lib/mc-status-report/throttle`; (3) la versión salía con texto largo → `sed` para limpiarla.

## Mundos del usuario migrados
- Se subieron `world.zip`, `world_nether.zip` y `world_the_end.zip` desde Descargas (venían de un servidor **Spigot 26.2**; se actualizaron solos a Paper 26.3 sin errores)
- Mundo final: **1.7 GB** — overworld 340 region files, the_nether 55, the_end 241
- Estructura en MC 26.3: `world/dimensions/minecraft/<dim>/{data,entities,poi,region}`
- Arranque del servidor con el mundo nuevo: **9.161 s**

## Modo no-premium
- `online-mode=false` en server.properties para que entren jugadores sin cuenta premium
- Verificado: los 12 jugadores del mundo tienen UUIDs versión 3 (offline) → el mundo original ya venía en modo no-premium → **no se pierden inventarios, stats ni advancements**
- Riesgo: cualquiera con la dirección de playit puede entrar; opción pendiente: activar whitelist

## Optimización del sistema operativo (Ubuntu)
- **Tapa de la laptop:** nunca suspende — `/etc/systemd/logind.conf.d/99-minecraft-lid.conf` (HandleLidSwitch=ignore y variantes) + `systemctl mask` de sleep.target, suspend.target, hibernate.target, hybrid-sleep.target y suspend-then-hibernate.target
- **CPU governor** fijado en `performance` (servicio `cpu-performance`)
- **WiFi power save apagado** (servicio `wifi-powersave-off`)
- **Sysctl:** swappiness 10, vfs_cache_pressure 50, buffers de red 16M, tcp_fastopen 3
- **Servicios innecesarios deshabilitados:** ModemManager, packagekit, multipathd, apport, sysstat, open-iscsi, kdump, cloud-init → de 33 a 23 servicios activos
- Journald limitado a 200 MB
- Crafty con `LimitNOFILE=65535`

## Cambio de red WiFi
- El servidor pasó de la red `Teltulien` (2.4 GHz, señal -70/-85 dBm, IP 10.0.0.134) a la red **`Omni  5g`** (5 GHz, señal -37 dBm; ojo: el nombre lleva **DOS espacios** entre "Omni" y "5g")
- **IP nueva: `192.168.39.109`** — ahora está en la misma red que la laptop Windows del usuario (192.168.39.x). La IP vieja `10.0.0.134` ya no funciona
- Configurado con netplan (systemd-networkd + wpa_supplicant; NetworkManager NO está instalado)
- Copia de la config anterior en `/var/tmp/netplan-backup/` + script de reversión `/usr/local/sbin/netplan-rollback.sh`

### 7-oct-2026 — reconexión al internet nuevo
- Red actual: **`Tertulien 5G`** (5 GHz, señal -36 dBm), IP LAN **`10.0.0.134`**.
- Archivo netplan real: **`/etc/netplan/00-installer-config.yaml`**. Ojo: existe un `00-installer-config.ymal` (extensión con typo) que **netplan ignora** — se editó ese por error.
- **Causa del "sin internet":** el `.yaml` seguía con la red vieja `CLAROBK9HK`; el editado era el `.ymal`. **Fix:** escribir el bloque `wifis` correcto en el `.yaml` + `sudo netplan apply`.
- **Reglas YAML:** 1 espacio tras cada `:`, sangría de 2 espacios por nivel (nunca Tab).
- Verificado por SSH: `wlp0s20f3` UP, ping OK, servicios `active`, Paper corriendo, backup 1.7 GB.
- **Menú gráfico de WiFi instalado:** paquete `network-manager` (trae **`nmtui`**). Renderer de netplan = `NetworkManager`. Para conectar WiFi: **`sudo nmtui`** → "Activar una conexión". Respaldo del netplan previo: `/root/netplan-00.yaml.bak`.
- **Gotcha:** tras `netplan apply` con renderer NetworkManager, NM no arrancó solo → `sudo systemctl enable --now NetworkManager`. Queda `enabled` en el arranque. Conexión activa: `netplan-wlp0s20f3-Tertulien 5G`.
- **Clave de `sudo` del usuario `terrible`:** la misma del panel Crafty (ver arriba).

## Cómo se accede
| Quién | Dirección |
|---|---|
| Tú (dueño) | `100.107.196.60:25565` por Tailscale |
| LAN | `192.168.39.109:25565` |
| Amigos | Dirección pública de playit.gg (túnel activo, agent id 02c7fcc8-f5be-4e37-9ab0-37afea4ccdcc) |
| Panel de administración | https://192.168.39.109:8443 (LAN) o https://100.107.196.60:8443 (Tailscale) |

## Notas técnicas útiles
- La API de Crafty v2 está en `https://127.0.0.1:8443/api/v2`; login `POST /auth/login`, enviar comando a la consola `POST /servers/<id>/stdin` con body en texto plano, acciones `POST /servers/<id>/action/start_server|stop_server`, y `GET /servers/<id>/stats` (es GET, no POST).
- Crafty **no** auto-arranca el servidor MC al reiniciarse; por eso se activó `auto_start` en la tabla `servers` de la base SQLite.
- Contraseña del panel cambiada desde la default (la default tiene `$&%!@` que se rompen al copiar/pegar). El hash es argon2id.

## Granja de enderman — causa y solución (17 Sep 2026, madrugada)
- **El problema** ("no bajan tantos enderman como antes") era que **el cebo (endermite) había desaparecido**; sin cebo los enderman spawnean pero no caminan al borde ni caen.
- **Solución aplicada:** se restauró la dimensión `the_end` desde `C:\Users\terrible019\Downloads\world_the_end.zip` (74,7 MB, md5 `e02ccc873ceb1914314f90c008e20ec3`) → **241 regiones, 167 archivos de entidades, 345 MB**. El End anterior quedó respaldado en `/var/tmp/end_old` (350 MB) y la extracción en `/var/tmp/endrestore`.
- **El cebo** es el endermite **"pablo"** (con `PersistenceRequired: 1`, Health 8.0) montado en un **minecart**, en `X -10.49, Y 104.06-104.25, Z -246.51` (chunk (-1,-16) = bloque X -16..-1 / Z -256..-241, region `r.-1.-1.mca`, índice 543). Verificado en vivo: endermite 1, minecart 1, enderman 25.
- **Importante:** la granja **solo produce con un jugador presente en el End** (por `disable-world-ticking-when-empty: true`).
- **Aclaración:** la torre de `X 1096-1111, Z -202..-218` y la vía de raíles `X 1033-1105, Z -240..-191` NO son la granja (esa era otra construcción del End).

## Ajuste de spawning (17 Sep 2026)
- `bukkit.yml` → `ticks-per.monster-spawns: 4 → 1` (era 4× menos intentos de spawn; el valor vanilla de Paper es 1).
- `spigot.yml` → `entity-activation-range` final: **animals 64, monsters 96, raiders 64, villagers 64, misc 16, water 16, flying-monsters 32** (antes animals 32, monsters 48, raiders 32, villagers 32).
- `simulation-distance` se queda en **12** y `view-distance` en **16** (sin tocar). Motivo: el tope útil de activación es 96 porque `entity-tracking-range.monsters` es 96.
- Reinicio aplicado el 17 Sep 01:14:44 AST → `Done (8.774s)!`, 0 errores.
- **Medición con 1 jugador:** tick promedio 13-15 ms (mín 9-10, máx 28-49; un pico aislado de 251,9 ms), **TPS 20,0/20,0/20,0**, CPU del proceso 7%, RAM 2.876/7.208 MB, temps NVMe 34-37 / WiFi 46 / CPU 52-56 °C, **0 ERROR/SEVERE, 0 "Can't keep up", 0 watchdog**. Conclusión: el ajuste no costó nada.

## Pendiente / futuro
- [ ] Subir `spawn-limits.monsters` de 70 a 100-150 si se quieren más mobs (ofrecido, sin respuesta)
- [ ] Decidir el **asistente IA en el chat del servidor**: recomendado un daemon propio que lee `latest.log` y responde con `tellraw` vía la API de Crafty; alternativa: plugin Nexa AI Chat (declara 26.2)
- [ ] Activar el modo conservación de batería de Lenovo (`conservation_mode=0`; batería con 776 ciclos / 82% de salud)
- [ ] Activar whitelist (el servidor está en `online-mode=false`; cualquiera con la dirección de playit puede entrar con cualquier nombre)
- [ ] Opcional: actualizar Crafty 4.10.8 → 4.11.0 y Paper a la última build (Paper avisa "3 builds behind")
- [ ] Opcional: pre-generar the_end con Chunky si algún día hace falta
- [ ] Opcional: poner tope de gasto (Caps & Alerts) en Backblaze B2 para no recibir cargos por sorpresa (el espacio ya no es problema: con retención de 1 solo ocupa ~1,6 GB de 10 GB)

## Enlaces
- **Accesos: [[Servidor Minecraft - Accesos]]** (SSH, sudo, WiFi, panel)
- Relacionado: [[Homelab - Mini PCs]] (PaperMC en el homelab MS-01)
- Hub: [[Projects MOC]] · [[Nexus]]