# Radarr

## What it's used for

Movie library automation — same role as Sonarr, but for movies. Runs on the
worker machine.

**Not routed through Cloudflare Tunnel.**
This specific service isn't exposed, as this would be considered an administrative tool, not needing to be accessiable to everyone; but rather its accessiable/hosted on (http://worker-node:7878)

## What was redacted

- Real domain/hostname
- Real media library paths
- API key used for the Prowlarr integration
