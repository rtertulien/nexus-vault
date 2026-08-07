---
title: "Homelab - Setup Final"
date: 2026-07-03
status: activo
type: project
tags:
  - homelab
  - proxmox
  - nas
  - raid5
  - presupuesto
  - arr-stack
  - jellyfin
up:
  - "[[Nexus]]"
---
# Homelab — Setup Final (3 Jul 2026)

## Presupuesto: $3,670

| Componente                                        | Precio |
| ------------------------------------------------- | ------ |
| MS-01 i9-13900H / 32GB / 1TB (Amazon)             | $1,139 |
| Asustor Flashstor 6 FS6706T (Amazon)              | $500   |
| 3x WD Black SN850X 4TB con heatsink (Amazon)      | $1,800 |
| GL.iNet Flint 3 GL-BE9300 WiFi 7 OpenWRT (Amazon) | $210   |
| Monoprice Cat6 2ft pack 12 (Amazon)               | $21    |
|                                                   |        |
|                                                   |        |

## Hardware

### MS-01 — Homelab principal
- Intel Core i9-13900H (vPro/AMT incluido — no necesita JetKVM)
- 32GB DDR5 (expandible a 64GB cuando bajen precios)
- 1TB SSD NVMe (pool local Proxmox)
- 2x 10GbE SFP+, 2x 2.5GbE RJ45, PCIe x16
- SO: Proxmox VE 8.x

### FS6706T — NAS all-flash
- 6 bahias M.2, PCIe 3.0 x1 por slot
- Dual 2.5GbE, 4GB DDR4
- SO: ADM (Asustor Data Manager)
- RAID 5: 3x SN850X 4TB = 8TB usables (3 slots libres para expandir)

### Red — Topologia

```
Claro Fibra (HG8145V5)
    |, DMZ activada (no bridge, bloqueado)
    V
GL.iNet Flint 3 (OpenWRT)
    |, WAN: PPPoE + VLAN 100, MTU 1492
    |, LAN: 192.168.x.x, AdGuard Home como DNS primario
    V
Flint 3 LAN
    |--- MS-01 (2.5GbE)
    |--- FS6706T NAS (2.5GbE)
    '--- futuro: switch cuando se necesiten mas puertos
```

#### Detalles Claro RD
- **Modem:** HG8145V5 (bloqueado, sin superadmin)
- **Bridge mode:** No disponible (Claro no lo permite)
- **DMZ:** Activada desde interfaz normal hacia el Flint 3
- **PPPoE:** usuario/contrasena = 6370141971, VLAN 100, MTU 1492

#### Plan futuro (1 Gbps simetrico)
- Migracion a XGS-PON cuando este disponible en la zona
- Nuevo modem WiFi 6 incluido con el upgrade
- Misma DMZ + Flint 3, solo cambia el modem de Claro

## Contenedores (8 total, ~27GB usados de 32GB)

| #   | Tipo | Servicio                | RAM         | Notas                                                     |
| --- | ---- | ----------------------- | ----------- | --------------------------------------------------------- |
| 1   | VM   | Docker arr stack        | **6GB**     | Radarr, Sonarr, Prowlarr, qBittorrent, Bazarr, Jellyseerr |
| 2   | VM   | Docker Immich           | **4GB**     | PostgreSQL + ML, app fotos familiar                       |
| 3   | LXC  | Jellyfin (privilegiado) | 4GB         | GPU QuickSync via /dev/dri                                |
| 4   | LXC  | AdGuard Home            | 512MB       | DNS blocker                                               |
| 5   | LXC  | Navidrome               | 512MB       | Streaming música                                          |
| 6   | LXC  | Tailscale               | 256MB       | VPN mesh                                                  |
| 7   | LXC  | Caddy                   | 256MB       | Reverse proxy HTTPS                                       |
| 8   | VM   | PaperMC                 | 6GB         | Minecraft (Java 25, Aikar's flags)                        |
|     |      | **Total apps**          | **~21.5GB** |                                                           |
|     |      | Proxmox host            | ~2.5GB      | OS + servicios base                                       |
|     |      | ZFS ARC (limitado)      | ~3GB        | 10% de 32GB, fijado manualmente                           |
|     |      | **Total**               | **~27GB**   |                                                           |
|     |      | **Libre**               | **~5GB**    | Colchón para picos y futuros servicios                    |

### Cambios de diseño

| Fecha | Cambio | Razón |
|------|--------|-------|
| 2 Jul 2026 | Plan inicial con CasaOS | — |
| 4 Jul 2026 | CasaOS → VM Docker arr stack (4GB) | Foros confirmaron CasaOS limitado para stacks complejos, networking Gluetun roto |
| 4 Jul 2026 | arr stack 4GB → 6GB | VM overhead ~1GB + qBittorrent picos con 100+ torrents |
| 4 Jul 2026 | PhotoPrism → Immich | Immich tiene app nativa iOS/Android con auto-backup, búsqueda semántica IA |
| 4 Jul 2026 | Nextcloud eliminado | Cubierto por NAS (archivos) + Immich (fotos) |
| 5 Jul 2026 | Immich 3GB → 4GB | Docs oficiales piden 6GB min, comunidad reporta 4-6GB para 10-20K fotos |
| 5 Jul 2026 | Libre final: 5GB | Colchón saludable verificado en r/Proxmox |
| 5 Jul 2026 | VPN elegida: ProtonVPN Plus | WireGuard + port forwarding, 29 servidores RD, $4.99/mes plan 2 años |

### Apps clave
- **Docker VM (arr stack):** Radarr (peliculas), Sonarr (series), Prowlarr (indexer), qBittorrent (torrents), Bazarr (subtitulos), Jellyseerr (catalogo)
- **Immich VM:** Immich (fotos/videos con app nativa iOS/Android)
- **Standalone:** Jellyfin, Navidrome, AdGuard, PaperMC, Tailscale, Caddy
- **VPN:** ProtonVPN Plus ($4.99/mes, plan 2 años). WireGuard + port forwarding. 29 servidores en RD.
- **Musica FLAC:** Soulseek (Nicotine+) desde laptop + NAS + Navidrome + iPhone (Amperfy)
- **Flujo:** Prowlarr + Sonarr/Radarr + qBittorrent + Jellyfin + Navidrome + TVs/moviles

## Router — GL.iNet Flint 3 (OpenWRT)
- Bridge mode Claro RD
- PPPoE + VLAN 100
- MTU WAN 1492, MTU LAN 1472
- Credenciales = numero telefono
- AdGuard Home como DNS primario

## Backups
- PBS (Proxmox Backup Server) diario 4AM + NAS
- **Futuro cercano:** Crucial X9 4TB USB-C conectado al MS-01 para backups PBS externos (no depender solo del NAS)
- **Futuro:** rclone a Backblaze B2 para datos criticos (3-2-1)

## Referencias
- [TRaSH Guides](https://trash-guides.info/) — configs de referencia para arr stack (hardlinks, carpetas, quality profiles, custom formats)
- [Proxmox FAQ 2026](https://pve.proxmox.com/wiki/FAQ) — Docker en VM vs LXC recomendaciones

## Diferido (sin fecha)
- **GMKtec M6 Ultra** — AI local (Ollama + agentes). MS-01 corre agentes en CPU mientras tanto. Pendiente re-evaluar cuando homelab este estable.
- **UPS APC (~$100-150)** — prioridad #1 para proteger el RAID de apagones
- **Crucial X9 4TB (~$372)** — SSD externo USB-C 1050MB/s para backups PBS. Prioridad baja-media, comprar cuando el homelab este corriendo y estable.
- **Switch NICGIGA 10-port (~$112)** — Prioridad baja. El Flint 3 tiene 4 LAN 2.5G (MS-01 + NAS sobran 2). Comprar solo si se necesitan mas puertos o el 10G.
- RAM 64GB (cuando bajen precios DDR5)

## Riesgos y mitigaciones

| Punto                   | Ya contemplado?                    | Status                 |
| ----------------------- | ---------------------------------- | ---------------------- |
| ZFS ARC come RAM        | Si — limitado a 3GB manualmente    | Resuelto               |
| SSD 1TB sin redundancia | Si — PBS diario al NAS mitiga      | Aceptado               |
| UPS                     | Si — apuntado como prioridad #1    | Diferido (~$100-150)   |
| 3-2-1 backup            | Si — rclone + B2 planeado          | Diferido               |
| ADM vs DSM              | Si — irrelevante para solo storage | Aceptado               |

## Notas/Observaciones

- TrueNAS no necesario — FS6706T usa ADM propio.
- community-scripts.org agregado como referencia de scripts Proxmox.
