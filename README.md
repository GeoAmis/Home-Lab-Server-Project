# Home Lab Server Project

> Welcome to my home lab project! This repo is where I document the build, configuration, and ongoing evolution of my personal server setup. It’s powered by Proxmox, TrueNAS, and a mix of hand-picked hardware.<br> 
The goal? To create a flexible, self-hosted environment for running VMs, containers, and managing data — and to learn a ton in the process.

## 🖥️ Hardware Overview
Here's what the server is running:
| Component  |  Details |
| ------------- |:-------------:|
|CPU            | Ryzen 7 5800X (8c/16t) |
| Motherboard   | B550 chipset ATX board |
| RAM           | 64Gb DDR4-3600Mhz (2x 32Gb) |
| GPU           | GTX 1050Ti 4GB |
| Boot Storage  | 2x 240GB SATA SSDs (RAID-1 for Proxmox) |
| Config Storage| 1x 128GB NVMe SSD (Fast storage) |
| HBA Card      | 16-port SAS HBA (12Gbps, PCIe 3.0 x8) |
| Data Storage  | 4x 4TB Enterprise HDDs (RAID-Z1 in TrueNAS) |
| Power Supply  | 650W modular |

## ⚙️ Virtualization & Storage Layout
**Hypervisor:** Proxmox VE<br>
**NAS:** TrueNAS Scale<br>
**Backup:** Proxmox Backup Server

## 💽 Storage Breakdown
* **Boot & Hypervisor:** Mirrored SATA SSDs used exclusively by Proxmox.<br>
* **Config Storage (NVMe SSD):** Used for fast access to configuration files and important Proxmox or container data. This is backed up regularly to TrueNAS to ensure it's recoverable if the drive fails.<br>
* **Data Storage (HDDs in TrueNAS):** 4x 4TB drives in a RAID-Z1 ZFS pool, passed through to TrueNAS via the HBA. This is the primary data storage location for the homelab.
* **NFS Integration:** TrueNAS exports two dedicated datasets (I'm giving placeholder names for easier follow up) via NFS or Samba, depending on the target system. These are mounted either on the Proxmox host or a Windows 11 machine, depending on use case:
  * **homelab-data:** Used for general-purpose files, LXC container volumes, and shared assets across the lab.
  * **proxmox-backups:** Serves as the backup target for the Proxmox Backup Server VM.<br>

This setup provides centralized, network-accessible storage for both virtualization and desktop environments, with a clear separation between operational data and system backups.

## 🗂️ Backup Strategy
Resilience is built into the system with a layered backup approach:
* **PBS VM** handles scheduled backups of all critical VMs and containers.
* **Configuration data** stored on the NVMe is regularly backed up to TrueNAS via Proxmox Backup.
* TrueNAS pools benefit from ZFS snapshots for data versioning and recovery.

## 🚀 Planned Enhancements
Here’s what I’m planning to add as the project evolves:

* **LXC Container Overview:** Breakdown of all running containers, their purposes (e.g. media, monitoring, utility), and how they’re configured.
* **Network & Storage Diagrams:** Visual representations of the system layout — including VM interconnections, network topology, storage flows, and backup paths.
* **Metrics & Dashboards:** Integration with monitoring tools like Netdata, Grafana, or Prometheus to track system health, performance, and resource usage in real time.
* **Configuration Management (Possibly GitOps):** Exploring the idea of managing container, VM, and storage configs via Git. May include basic versioning, scripts, or potentially a GitOps-like structure — depending on complexity and usefulness.

## 🧠 Why I'm Doing This
This project is a hands-on, self-hosted homelab — part learning platform, part utility server. It’s designed to be modular, resilient, and educational. I use it to explore virtualization, ZFS, backup strategies, containerization, document the entire journey in a reproducible way, and more.<br>
If you’re into Proxmox, ZFS, or homegrown infrastructure — welcome!

## 🤝 Contributions & Feedback
Feel free to open an issue or request if you’ve got suggestions, similar setups, or ideas. I’m always open to tips from the homelab community!
