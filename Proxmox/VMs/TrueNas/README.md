# 🗄️ TrueNAS VM Setup
This section documents the configuration and role of the TrueNAS Scale virtual machine running inside the homelab.

## 💡 Overview
The TrueNAS VM acts as the primary storage backend for the homelab. It runs on Proxmox and is responsible for managing data storage, sharing, and ZFS-level protection. It also serves as the NFS backend for container volumes and backups.

## 🔧 VM Configuration
| Component  | 	Details |
| ------------- |:-------------:|
| CPU           | 4 cores (assigned from Proxmox) |
| RAM           | 32 GB |
| Storage       | Direct passthrough of the HBA card, exposing 4x 4TB enterprise HDDs |
| OS            | TrueNAS SCALE |

## 💽 Storage Layout
A **RAID-Z1 ZFS pool** is created inside TrueNAS using the 4x 4TB enterprise HDDs passed through via the HBA card. This pool is used for persistent and resilient data storage across the lab.

Two primary datasets have been created (I'm giving placeholder names for easier follow up):

* **`homelab-data`**  
  General-purpose storage for files, shared assets, and LXC container volumes.

* **`proxmox-backups`**  
  Dedicated target for the Proxmox Backup Server VM, mounted via NFS.

Both datasets are exported via **NFS or Samba**, depending on the target client (Proxmox or Windows). This gives maximum flexibility while keeping data logically separated.

## 📌 Future Plans
* **Automated Snapshots & Retention Policies**<br>
  Set up scheduled ZFS snapshots with clean retention rules for versioning and easy recovery of lab data.
* **Performance Tuning**<br>
  Experiment with SSD-based caching (L2ARC or ZIL/SLOG) to improve I/O performance for frequent-access or write-heavy workloads.
* **Monitoring Integration**<br>
  Pull ZFS and system metrics into Grafana/Netdata for better observability of disk health, usage trends, and performance.
