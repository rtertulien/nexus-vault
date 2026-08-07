---
title: "Homelab - Docker Compose"
date: 2026-07-04
status: activo
type: reference
tags: [homelab, docker, compose, arr-stack, gluetun, vpn, trash-guides]
up: [[Homelab - Mini PCs]]
---

# Docker Compose — Arr Stack

> Versión final. Arquitectura: solo qBittorrent detrás de Gluetun VPN.
> Arrs en red normal, conectan a qBittorrent vía hostname `gluetun:8080`.
> TRaSH Guides compliant con hardlinks.

## Estructura de carpetas (crear antes de levantar compose)

```bash
mkdir -p /data/{torrents/{tv,movies,music},media/{tv,movies,music}}
mkdir -p /docker/appdata/{gluetun,qbittorrent,prowlarr,radarr,sonarr,bazarr,jellyseerr}
chown -R 1000:1000 /data /docker/appdata
```

> `/data/torrents` y `/data/media` deben estar en el MISMO punto de montaje NFS para que los hardlinks funcionen (no duplican espacio).

## docker-compose.yml

```yaml
x-common-env: &common-env
  PUID: 1000
  PGID: 1000
  TZ: America/Santo_Domingo
  UMASK: "002"

services:

  ############################################################
  # GLUETUN — VPN gateway (ProtonVPN, solo qBittorrent pasa por acá)
  ############################################################
  gluetun:
    image: qmcgaw/gluetun:latest
    container_name: gluetun
    restart: unless-stopped
    cap_add:
      - NET_ADMIN
    devices:
      - /dev/net/tun:/dev/net/tun
    ports:
      - "8080:8080"       # WebUI qBittorrent
      - "6881:6881"       # torrent TCP
      - "6881:6881/udp"   # torrent UDP
    environment:
      - VPN_SERVICE_PROVIDER=${VPN_SERVICE_PROVIDER}
      - VPN_TYPE=wireguard
      - VPN_PORT_FORWARDING=${VPN_PORT_FORWARDING}
      - WIREGUARD_PRIVATE_KEY=${WG_PRIVATE_KEY}
      - WIREGUARD_ADDRESSES=${WG_ADDRESSES}
      - SERVER_COUNTRIES=${VPN_COUNTRY}
      - FIREWALL_OUTBOUND_SUBNETS=${LAN_SUBNET}
      - TZ=America/Santo_Domingo
    volumes:
      - /docker/appdata/gluetun:/gluetun
    healthcheck:
      test: ["CMD", "wget", "-qO-", "https://api.ipify.org"]
      interval: 30s
      timeout: 10s
      retries: 3
    networks:
      - media

  ############################################################
  # QBITTORRENT — vive dentro de la red de Gluetun
  ############################################################
  qbittorrent:
    image: ghcr.io/hotio/qbittorrent:latest
    container_name: qbittorrent
    restart: unless-stopped
    network_mode: "service:gluetun"
    depends_on:
      gluetun:
        condition: service_healthy
    environment:
      <<: *common-env
    volumes:
      - /docker/appdata/qbittorrent:/config
      - /data:/data

  ############################################################
  # PROWLARR — red normal (solo busca metadata)
  ############################################################
  prowlarr:
    image: ghcr.io/hotio/prowlarr:latest
    container_name: prowlarr
    restart: unless-stopped
    ports:
      - "127.0.0.1:9696:9696"
    environment: *common-env
    volumes:
      - /docker/appdata/prowlarr:/config
    networks:
      - media

  ############################################################
  # RADARR — películas
  ############################################################
  radarr:
    image: ghcr.io/hotio/radarr:latest
    container_name: radarr
    restart: unless-stopped
    depends_on:
      - qbittorrent
    ports:
      - "127.0.0.1:7878:7878"
    environment: *common-env
    volumes:
      - /docker/appdata/radarr:/config
      - /data:/data
    networks:
      - media

  ############################################################
  # SONARR — series
  ############################################################
  sonarr:
    image: ghcr.io/hotio/sonarr:latest
    container_name: sonarr
    restart: unless-stopped
    depends_on:
      - qbittorrent
    ports:
      - "127.0.0.1:8989:8989"
    environment: *common-env
    volumes:
      - /docker/appdata/sonarr:/config
      - /data:/data
    networks:
      - media

  ############################################################
  # BAZARR — subtítulos automáticos
  ############################################################
  bazarr:
    image: ghcr.io/hotio/bazarr:latest
    container_name: bazarr
    restart: unless-stopped
    ports:
      - "127.0.0.1:6767:6767"
    environment: *common-env
    volumes:
      - /docker/appdata/bazarr:/config
      - /data:/data
    networks:
      - media

  ############################################################
  # JELLYSEERR — portal de peticiones
  ############################################################
  jellyseerr:
    image: fallenbagel/jellyseerr:latest
    container_name: jellyseerr
    restart: unless-stopped
    ports:
      - "127.0.0.1:5055:5055"
    environment: *common-env
    volumes:
      - /docker/appdata/jellyseerr:/app/config
    networks:
      - media

networks:
  media:
    name: media_network
```

## .env

```
# ProtonVPN
VPN_SERVICE_PROVIDER=protonvpn
VPN_COUNTRY=Netherlands
WG_PRIVATE_KEY=
WG_ADDRESSES=
LAN_SUBNET=192.168.8.0/24
VPN_PORT_FORWARDING=on
```

## ⚠️ Punto crítico (bug de conectividad evitado)

En Radarr/Sonarr → Settings → Download Clients → qBittorrent:

- **Host:** `gluetun` (NO localhost, NO qbittorrent)
- **Port:** `8080`

qBittorrent vive dentro de la red de Gluetun, no tiene IP propia. Desde los arrs (red normal) se resuelve el hostname `gluetun`.

## Flujo completo

```
Jellyseerr (pides contenido)
  → Radarr/Sonarr (buscan en Prowlarr)
    → qBittorrent (descarga vía Gluetun VPN)
      → /data/torrents/{tv,movies,music}
        → Radarr/Sonarr hardlink a /data/media/{tv,movies,music}
          → Bazarr agrega subtítulos
            → Jellyfin sirve /data/media a TVs/móviles
```

## AdGuard — dominios a whitelist

```
skyhook.sonarr.tv
api.themoviedb.org
thetvdb.com
```

## Navidrome (LXC aparte)

```yaml
volumes:
  - /data/media/music:/music:ro
```

Montar `/data/media/music` como solo lectura desde el LXC de Navidrome.

## Referencias
- TRaSH Guides: https://trash-guides.info/
- hotio images: https://hotio.dev/
- Gluetun: https://github.com/qdm12/gluetun
