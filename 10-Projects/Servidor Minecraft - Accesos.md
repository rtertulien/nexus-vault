---
type: resource
status: activo
up: "[[Servidor Minecraft]]"
tags: [servidor-minecraft, accesos, credenciales, ssh, mc-server, tailscale, crafty]
created: 2026-10-07
related: ["[[Servidor Minecraft]]"]
---

# Servidor Minecraft — Accesos

> Datos de acceso a la laptop del servidor (`mc-server`). **No volver a pedir usuario/contraseña.**

## SSH (desde el PC Windows)
- Llave: `C:\Users\terrible019\.ssh\mc_backup`
- Comando:
```powershell
ssh -i C:\Users\terrible019\.ssh\mc_backup terrible@100.107.196.60
```
- Usuario: `terrible`
- Hostname: `mc-server`
- IP Tailscale: `100.107.196.60`
- IP LAN: `10.0.0.134`
- **Contraseña de `sudo`: `colombia2811`** (la misma del panel Crafty)

## WiFi
- Red: `Tertulien 5G` · Clave: `Terrible019`
- Conectar/cambiar con menú: `sudo nmtui` → "Activar una conexión"
- Netplan: `/etc/netplan/00-installer-config.yaml` (`renderer: NetworkManager`)
- Respaldo netplan: `/root/netplan-00.yaml.bak`

## Panel Crafty
- URL: https://100.107.196.60:8443 (Tailscale) · https://10.0.0.134:8443 (LAN)
- Usuario: `admin` · Contraseña: `colombia2811`

## Servicios
`NetworkManager` · `crafty` · `playit` · `tailscaled` · `ssh` · `mc-dashboard`

## Otros datos
- Crafty Server ID: `3c290914-d463-4e38-b896-3f1dbce26b5c`
- Bucket Backblaze B2: `mc-backups-rt2811`
- Backup local del mundo: `C:\Users\terrible019\MC-Backups\`

## Enlaces
- Proyecto: [[Servidor Minecraft]]
