# qBittorrent

## What it's used for

Download client that Sonarr and Radarr push completed grabs to; the actual
downloading component of the automation setup (Prowlarr finds it, Sonarr/Radarr
manage it, qBittorrent downloads it). Runs on the worker machine.

**Not routed through Cloudflare Tunnel.**
This specific service isn't exposed, as this would be considered an administrative tool, not needing to be accessiable to everyone; but rather its accessiable/hosted on (http://worker-node:8080) or authenticated users via Tailscale

## What was redacted

- Real download/library paths
- WebUI credentials