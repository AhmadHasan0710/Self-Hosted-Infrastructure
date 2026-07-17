# Monitors Configured

| Monitor       | Target                                | Type |

| Website       | https://example.com                   | HTTP(s) |
| Jellyfin      | https://jellyfin.example.com          | HTTP(s) |
| Jellyseerr    | https://requests.example.com          | HTTP(s) |
| Nextcloud     | https://nextcloud.example.com         | HTTP(s) |
| Prowlarr      | http://WorkerIP:port.com              | HTTP |
| Sonarr        | https://WorkerIP:port.com             | HTTP |
| Radarr        | https://WorkerIP:port.com             | HTTP |
| qBittorrent   | http://workerIP:8090 (via Tailscale)  | HTTP |

Each monitor hits the **public hostname**, not localhost — this validates the
entire path (Cloudflare -> Tunnel -> service), not just that the container is
running locally.

Notifications are routed thru my personal Gmail via a SMTP Server hosted through the application.
