# ⚙️ Proxmox Virtualization Platform

>This section documents the configuration and role of the Proxmox VE hypervisor in my homelab.

## 💡 Overview

Proxmox VE is the virtualization backbone of the homelab, running directly on bare metal. It orchestrates lightweight LXC containers (with Docker) and full VMs to manage media, storage, development tools, and monitoring services.

## 🖥️ System Overview

| Component        | Details                                                                 |
|------------------|-------------------------------------------------------------------------|
| Hypervisor       | Proxmox VE (bare metal)                                                 |
| Boot Storage     | 2× 240GB SATA SSDs (RAID-1 for Proxmox system)                          |
| Config Storage   | 1× 128GB NVMe SSD (`ct-config` stores container configs; backed up regularly)       |
| Shared Storage   | `homelab-data` (NFS from [TrueNAS](VMs/TrueNAS)) for persistent volumes |
| Backup Storage   | `proxmox-backups` (NFS from [TrueNAS](VMs/TrueNAS)) for backups via PBS |
| Network Setup    | Linux bridges with optional VLAN support                                |

## 📦 Containers & Virtual Machines

All LXC containers use the NVMe SSD for their configuration and mount `homelab-data` for persistent storage needs.

| Name                           | Type | Description                                                                          | Resources         | Storage                        | Status              |
|--------------------------------|-----|---------------------------------------------------------------------------------------|-------------------|--------------------------------|---------------------|
| [`media`](CTs/media)           | LXC | Docker-based media stack (Jellyfin, Sonarr, Radarr, etc.)                             | 4 cores<br> 8GB RAM  | `ct-config`<br> `homelab-data` | ✅ Running          |
| [`cloud`](CTs/cloud)           | LXC | Nextcloud, Immich, and cloud-like services.                                           | 2 cores<br> 4GB RAM  | `ct-config`<br> `homelab-data`          | ✅ Running          |
| [`dns`](CTs/dns)               | LXC | Pi-hole + Nginx Proxy Manager for local DNS and reverse proxy .                       | 2 cores<br> 4GB RAM  | `ct-config`<br> `homelab-data`          | ✅ Running          |
| [`monitoring`](CTs/monitoring) | LXC | Prometheus, Grafana, Netdata, BeZsel, etc.                                            | 2 cores<br> 2GB RAM  | `ct-config`<br> `homelab-data`          | ✅ Running          |
| [`games`](CTs/games)           | LXC | Minecraft, Valheim and other lightweight game servers.                                | 4 cores<br> 16GB RAM | `ct-config`<br> `homelab-data`          | ✅ Running          |
| [`code`](CTs/code)             | LXC | Dev tools (VSCode Server, Git, etc.).                                                 | TBD               | `ct-config`<br> `homelab-data`          | 🛠️ Work in Progress |
| [`home-automation`](CTs/home-automation) | LXC | Home Assistant, ESPHome, Zigbee2MQTT, etc.                                  | TBD               | `ct-config`<br> `homelab-data`          | 🛠️ Work in Progress |
| [`TrueNAS`](VMs/TrueNAS)       | VM  | ZFS-based NAS with HBA passthrough, exports datasets over NFS/Samba.                  | 4 cores<br> 32GB RAM | Physical disks                 | ✅ Running          |
| [`Proxmox Backup Server`](VMs/Proxmox-Backup-Server)| VM | Proxmox Backup Server, backs up VMs and CTs to `proxmox-backups`. | 2 cores<br> 8GB RAM  | `proxmox-backups` (NFS from [TrueNAS](VMs/TrueNAS)) |✅ Running          |
| [`Windows 11`](VMs/Windows11)  | VM  | Desktop OS for remote access and app testing.                                         | 4 cores<br> 8GB RAM  | TBD                            | ✅ Running          |
| [`Ubuntu`](VMs/Ubuntu)         | VM  | General-purpose Linux environment.                                                    | 2 cores<br> 4GB RAM  | TBD                            | ✅ Running          |

> 🔗 _Each link will eventually point to a detailed page explaining the setup, purpose, Docker Compose (if applicable), and special configs for that container or VM._

## 🔐 Backup Strategy

- All VMs and CTs are backed up via the [`PBS`](VMs/Proxmox-Backup-Server) VM to the `proxmox-backups` dataset.
- The NVMe SSD — which stores container configurations (`ct-config`) — is included in backup jobs.
- TrueNAS uses ZFS snapshots for additional protection of data volumes on `homelab-data`.

## 🛠️ Planned Improvements

- Implement VLANs for logical isolation between lab zones
- GPU passthrough to accelerate media workloads
- Git-based configuration versioning for reproducibility
- More automation through shell scripts or light GitOps workflows
