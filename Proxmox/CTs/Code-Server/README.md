# Code-Server (VS Code in the Browser)

This container runs [code-server](https://github.com/coder/code-server) — a web-based version of Visual Studio Code — served securely inside a Proxmox LXC container. It enables you to write, edit, and deploy code directly from your browser.

## 🐳 Container Overview
| Component     | Value                        |
|---------------|------------------------------|
| Image         | `lscr.io/linuxserver/code-server:latest` |
| Port          | `8443` (or `${CODE_SERVER_PORT}` from `.env`) |
| UI Access     | Web browser via HTTPS |
| Storage       | Persistent `/config` and project workspace volumes |
| Authentication| Web password login or reverse proxy auth |

## ⚙️ Environment Configuration

This container uses a `.env` file for dynamic configuration. Customize the values in the `.env` file to match your user, paths, and secrets.

### Required:

| Variable            | Description |
|---------------------|-------------|
| `PUID`, `PGID`      | Linux UID/GID for permissions |
| `TZ`                | Timezone (e.g., `Europe/Berlin`) |
| `PASSWORD`          | Web UI login password |
| `CONFIG_PATH`       | Path to store code-server config and extensions |
| `WORKSPACE_PATH`    | Host directory containing your code projects |
| `DEFAULT_WORKSPACE` | Default open folder inside container |
| `CODE_SERVER_PORT`  | Port for accessing the UI |
| `PROXY_DOMAIN`      | Your domain (e.g., `code.yourdomain.com`) for reverse proxy setup |

### Optional:

| Variable        | Description |
|-----------------|-------------|
| `PWA_APPNAME`   | Name shown for Progressive Web App installs |
| `HASHED_PASSWORD` | Pre-hashed login password using bcrypt |
| `SUDO_PASSWORD` / `SUDO_PASSWORD_HASH` | Set for enabling sudo access in the container |

## 📁 Folder Mapping

| Host Path (Proxmox)        | Container Path         | Purpose                    |
|----------------------------|------------------------|----------------------------|
| `${CONFIG_PATH}`           | `/config`              | User settings, extensions  |
| `${WORKSPACE_PATH}`        | `${DEFAULT_WORKSPACE}` | Code/projects directory    |

> Ensure both host paths are created and owned by the correct PUID/PGID before deploying.

## ⚙️ Requirements
- [Docker](https://docs.docker.com/get-docker/)
- [Docker Compose](https://docs.docker.com/compose/)<br>
- [Portainer (optional)](https://www.portainer.io/) – A web-based Docker management UI. You can use this to visually manage containers and volumes.
> Make sure they are installed before proceeding.

## 📦 Deploy Instructions
From the same directory as your `docker-compose.yaml`:
```
docker compose pull       # Fetch the latest image
docker compose up -d     # Start the service in the background
docker compose down      # Stop and remove the container
```

## 🌐 Accessing the Code Editor
Once deployed, access the code-server UI at:<br>
`https://<CT-IP>:${CODE_SERVER_PORT}`<br>
If using a reverse proxy:<br>
`https://${PROXY_DOMAIN}`
> 🔐 Place behind a reverse proxy with HTTPS and external authentication if exposing to the internet.

## 🧠 Tips
* All settings, extensions, and user data persist in the `/config` directory.
* The workspace folder maps directly to your host project directory.
* Use `.env` to easily customize ports, paths, and credentials.
* Mount your real project folder as the workspace for seamless editing.
* Prefer `HASHED_PASSWORD` over `PASSWORD` for stronger security.
* Enable `SUDO_PASSWORD` if you need root privileges inside the container.
* Use a reverse proxy to enforce HTTPS and centralize authentication.
