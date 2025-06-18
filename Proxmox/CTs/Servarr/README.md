# 📦 Servarr Stack (LXC Container)
>This container hosts a full media automation suite, routed securely through a VPN (Gluetun), and orchestrated via Docker Compose with Portainer as an optional GUI. It runs on Proxmox as a Docker LXC container.

## ⚙️ Overview
- **Container Type:** LXC (on Proxmox)
- **Purpose:** Automate downloading, organizing, and streaming media
- **Docker Management:** Portainer (accessible via web UI)
- **Reverse Proxy:** Integrated via [dns](../dns) container using Nginx Proxy Manager
- **Monitoring:** Externally handled via [monitoring](../monitoring)
- **Status:** ✅ Running

## 📁 Storage & Configuration
| Item             | Location / Notes                                                |
|------------------|------------------------------------------------------------------|
| **Media Data**   | Mounted from `homelab-data` dataset via NFS from [TrueNAS](../../VMs/TrueNAS) |
| **Configs**      | Stored in `ct-config` (mounted from NVMe SSD)                   |
| **Environment**  | Configurable through `.env` file                                |
| **VPN Traffic**  | Routed via Gluetun container                                    |

## ⚙️ Stack Components
| Service       | Description                               |
|---------------|-------------------------------------------|
| Gluetun       | VPN gateway for all other containers      | 
| qBittorrent   | Torrent client                            | 
| Sonarr        | TV Show downloader                        |
| Radarr        | Movie downloader                          |
| Prowlarr      | Indexer manager for Sonarr/Radarr         |
| Bazarr        | Subtitle fetcher                          |
| Jellyfin      | Media server                              |
| Jellyseerr    | Media request frontend                    |
| FlareSolverr  | Captcha bypass service                    |
| Homepage      | Dashboard to unify service access         |
| Portainer     | Docker container management UI            |

> All services are routed via Gluetun using `network_mode: service:gluetun`, so only ports exposed in Gluetun are accessible.

## 🛠️ Setup Instructions

1. **Edit `.env`**
  * Add your timezone
  * VPN credentials
  * Volume paths for configs (`CONFIG_DIR`) and media (`DATA_DIR`)
  * Your own Ports

2. **Review and Deploy with Docker Compose** <br>
   on the console run `docker compose up -d`

## 🌐 Access Services
Access available services via:
- `http://<IP>:3000` — Homepage dashboard  
- `http://<IP>:8989` — Sonarr  
- `http://<IP>:7878` — Radarr  
- _...and more depending on your stack._

> All routes are proxied securely through the Gluetun VPN container. Replace the `<IP>` with the Container's IP.

## 🔒 VPN Architecture
This stack leverages **Gluetun** to route all network traffic from your services through a secure VPN tunnel using **WireGuard**.  
No container has direct internet access outside the tunnel.

## 📂 File Structure
| File / Folder         | Purpose                                            |
|-----------------------|----------------------------------------------------|
| `docker-compose.yaml`| Docker Compose config for the full stack.|
| `.env` | Environment variables and secrets ( in order for it to work must be either in the same folder as the `docker-compose.yaml` file, or find in the yaml file the .env and change it into your own path).|

## 📌 Notes
- Media files are stored in `/data`, which maps to the **`homelab-data`** NFS share from [TrueNAS](../../VMs/TrueNAS).
- All configuration files for containers are stored in `/config` on the **`ct-config`** NVMe SSD.
- Reverse proxy is handled by the [`dns`](../dns) container using Nginx Proxy Manager.
- System monitoring is handled externally via the [`monitoring`](../monitoring) container stack.

## 🔗 Related Resources
- [Containers](../CTs)
- [TrueNAS Setup](../../VMs/TrueNAS)
- [Proxmox Hypervisor Overview](../../README.md)
- [DNS / Reverse Proxy Stack](../dns)
- [Monitoring Stack](../monitoring)
