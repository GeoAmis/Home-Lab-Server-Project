# 📦 Proxmox Containers Overview
This directory contains all the containerized services (CTs) running on my Proxmox VE setup. Each CT is a lightweight LXC container running Docker with Ubuntu 22.04.5, providing isolated services for media, cloud, automation, and development needs.

## 🤔 Why Use Containers?

LXC containers are lightweight, fast to deploy, and resource-efficient compared to full VMs. They provide:
* 🧩 Modular service isolation
* ♻️ Quick rebuild and versioning
* 🛠️ Easy resource scaling
* ⚡ Low overhead on system resources
  
Each container in this repo runs a dedicated service stack using Docker Compose for simple orchestration.

## 🐳 Docker & OS Stack
All containers are based on:
* 🐧 **Ubuntu 22.04.5 LTS**
* 🐳 **Docker** & **Docker Compose**
* 📁 Mounted volumes for persistent config/data

Docker Compose files define multi-service stacks (e.g., Sonarr + Radarr + qBittorrent + Jellyfin) inside each CT.

## 📁 Folder Structure in Compose Files
Here's how directory bindings are used in each `docker-compose.yaml` file:

| Path Variable     | Host Location (Proxmox)       | Container Mount Point      | Description                          |
|-------------------|-------------------------------|----------------------------|--------------------------------------|
| `${CONFIG_DIR}`   | Stored on `ct-config` NVMe SSD | `/config`, `/app/config`   | Stores app settings & databases      |
| `${DATA_DIR}`     | NFS share from TrueNAS         | `/data`                    | Media libraries, downloads, assets   |

> Always mount config files to the NVMe (`ct-config`) and large data (media) to the shared NFS volume (`homelab-data`).

## 🌐 Port Bindings Explained
In Compose files, exposed ports follow this pattern:
```
ports:
  - ${PORT_SERVICE}:INTERNAL_PORT
```
* Left side (`${PORT_SERVICE}`): External port accessible on the network, defined in `.env` (you can change this to your needs)
* Right side (`INTERNAL_PORT`): The port the service listens to inside the container (should not be changed)

> Using variables makes it easy to update ports from a central place without touching Compose files.

## 📁 Container Services Directory
Each container stack (e.g. Servarr, Cloud, DNS, etc.) lives in its own folder under this directory, and includes:
* Full `docker-compose.yaml` setup
* `.env` with ports, credentials, and volumes
* A self-documented `README.md`

## 🧠 Using Portainer
Portainer is included in this setup to provide a web-based user interface for Docker management. It greatly simplifies tasks such as:
* Viewing running containers, volumes, networks
* Starting, stopping, and restarting services
* Pulling updated images
* Creating new containers without needing CLI commands
* Monitoring logs and container statistics

## 🖥️ Access Portainer:
  ```
  http://<LXC-IP>:9443
  ```
  > Replace <LXC-IP> with the IP address of the LXC container hosting Portainer.

## 🔐 Best Practices
* ✅ Always use official images or those from well-established sources (e.g. `linuxserver.io`, `ghcr.io`, `qmcgaw`)
* 🔄 Frequently check for upstream changes in services before pulling updates
* 🔧 Update `.env` files with your own secure values (e.g., keys, ports, paths)
* 🔍 Carefully review volumes and network configurations before deploying
* ❌ Avoid exposing sensitive apps directly to the internet without a reverse proxy and authentication

> Feel free to use or adapt these stacks for your own setup.
## 📥 How to Pull, Deploy & Stop Services
Standard Docker Compose lifecycle:
### 🔄 Pull latest images:
```
docker compose pull
```
### 🚀 Deploy containers:
```
docker compose up -d
```
### 🛑 Stop & remove services:
```
docker compose down
```
> Ensure you're in the same directory as the `docker-compose.yaml` when running these commands.

## 📌 Important Notes
* Each CT runs isolated services managed by Docker Compose inside Ubuntu
* Data is stored externally on TrueNAS (`/data`) and configs on NVMe SSD (`/config`)

## 🧭 Final Tips
* 📁 Use version control (e.g., Git) to track changes to your Compose stacks.
* 🧪 Test each change locally or in staging before applying to production setups.
* 🔐 Keep sensitive data (e.g., API keys, VPN keys) in .env files and never commit them to public repositories.
* ✅ Use `docker compose config` to validate your full config before launching.

## ⚠️ Disclaimer
* All service configurations and Compose files in this repository were tested and working as of the last update.
* Future updates of images or upstream services might break certain functionality.
* You are responsible for testing and adapting the configuration to your own environment and security requirements.

## 🔗 Related Resources
* [🧊 TrueNAS](../VMs/TrueNAS)
* [🖥️ Proxmox](../../Proxmox)
* [🌐 DNS / Reverse Proxy](/dns)
* [📊 Monitoring](/monitoring)

Happy containerizing! 🐳✨
