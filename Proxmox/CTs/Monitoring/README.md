# Monitoring Stack: Uptime Kuma + Beszel

This project sets up a lightweight, self-hosted monitoring stack using **Uptime Kuma** and **Beszel** via Docker Compose.

## 🔍 What This Stack Does
This stack helps you **monitor services and infrastructure** through:

- **Uptime Kuma**: A powerful self-hosted monitoring tool with a modern UI. It monitors websites, ports, Docker containers, and more.
- **Beszel**: A web interface for reading logs from various sources. Simple and container-friendly.

This setup provides both **status monitoring** and **log visibility**—essential for keeping your systems healthy.

## 🛠️ Why You Might Need This
- **Proactive Monitoring**: Know when your services go down before users do.
- **Log Visibility**: Quickly view service logs without SSH access.
- **Self-Hosted**: Full control over your monitoring and alerting.
- **Extensible**: Easy to integrate notifications (e.g., via Gotify, coming soon!).

## 🔍 How Each Service Works

### 🟢 Uptime Kuma – Service Monitoring Dashboard
- Periodically checks HTTP(S), TCP ports, pings, or DNS records.
- Stores status history and alert logs.
- Sends notifications via Email, Telegram, Gotify, etc.
- Can monitor Docker containers directly via Docker socket.

**Use Cases:**
- Monitor uptime of web APIs or internal services.
- Track latency and response times.
- Get alerts if a container goes down unexpectedly.

### 📈 Beszel – Real-Time Log Viewer
- Reads from mounted log directories (plaintext or JSON).
- Displays logs in a readable browser UI.
- **Live reloads** updates for real-time debugging.
- Excellent companion to Uptime Kuma for root cause analysis.

**Example Uses**:
- View application logs without SSH access.
- Debug failed services using timestamped logs.
- Point Beszel at any local or shared log directory you want to explore visually.

Together, these tools give you **visibility (Uptime Kuma)** and **insight (Beszel)** into the health of your systems.

## ⚙️ Requirements
- [Docker](https://docs.docker.com/get-docker/)
- [Docker Compose](https://docs.docker.com/compose/)<br>
- [Portainer (optional)](https://www.portainer.io/) – A web-based Docker management UI. You can use this to visually manage containers and volumes.
> Make sure they are installed before proceeding.

## 🚀 Getting Started
1. Edit the `.env` with paths/ports as needed.
2. Review `docker-compose.yaml` to understand what's being deployed.
3. Run the stack with:
```
docker compose pull      # Fetch the latest image
docker compose up -d     # Start the service in the background
docker compose down      # Stop and remove the container
```
## 🌐 Accessing the Monitors
Once deployed, access the monitors at::<br>
Uptime Kuma: `http://localhost:<UPTIME_KUMA_PORT>`<br>
Beszel: `http://localhost:<BESZEL_PORT>`

## 📣 Upcoming Features
- 🔔 Gotify integration for mobile/desktop alerts.
- 📊 **Optional** Prometheus and Grafana integration to enable comprehensive metrics collection and customizable dashboards — recommended for advanced monitoring and deep system visibility.
- 🔄 Scheduled backups of data volumes.
