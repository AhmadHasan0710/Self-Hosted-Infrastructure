# Monitors Configured

| Monitor     | Target                                 | Type    |
|-------------|----------------------------------------|---------|
| Website     | `https://example.com`                  | HTTP(s) |
| Jellyfin    | `https://jellyfin.example.com`         | HTTP(s) |
| Jellyseerr  | `https://requests.example.com`         | HTTP(s) |
| Nextcloud   | `https://nextcloud.example.com`        | HTTP(s) |
| Prowlarr    | `http://WorkerIP:port` (via Tailscale) | HTTP    |
| Sonarr      | `http://WorkerIP:port` (via Tailscale) | HTTP    |
| Radarr      | `http://WorkerIP:port` (via Tailscale) | HTTP    |
| qBittorrent | `http://WorkerIP:8090` (via Tailscale) | HTTP    |

Each monitor hits the **public hostname**, not localhost — this validates the
entire path (Cloudflare → Tunnel → service), not just that the container is
running locally.

Notifications are routed through my personal Gmail via an SMTP server hosted
through the application.