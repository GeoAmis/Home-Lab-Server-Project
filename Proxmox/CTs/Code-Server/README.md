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

### Optional:

| Variable        | Description |
|-----------------|-------------|
| `PROXY_DOMAIN`  | Domain for reverse proxy (e.g., `code.domain.com`) |
| `PWA_APPNAME`   | Name shown for Progressive Web App installs |

## 📁 Folder Mapping

| Host Path (Proxmox)        | Container Path         | Purpose                    |
|----------------------------|------------------------|----------------------------|
| `${CONFIG_PATH}`           | `/config`              | User settings, extensions  |
| `${WORKSPACE_PATH}`        | `${DEFAULT_WORKSPACE}` | Code/projects directory    |

> Ensure both host paths are created and owned by the correct PUID/PGID before deploying.

## 🌐 Accessing the Code Editor

Once deployed, access the code-server UI at:<br>
`https://<CT-IP>:${CODE_SERVER_PORT}`

If using a reverse proxy:<br>
`https://${PROXY_DOMAIN}`

> 🔐 Place behind a reverse proxy with HTTPS and external authentication if exposing to the internet.

## 📦 Deploy Instructions

From the same directory as your `docker-compose.yaml`:
```
docker compose pull       # Fetch the latest image
docker compose up -d     # Start the service in the background
docker compose down      # Stop and remove the container
```

## 🧠 Tips
* Code-server persists all settings and extensions in the /config directory.
* The workspace folder maps directly to your host project directory.
* You can install any VS Code extension from the Marketplace as usual.
* Use reverse proxy auth (like Authelia or OAuth2) for more secure access control.
