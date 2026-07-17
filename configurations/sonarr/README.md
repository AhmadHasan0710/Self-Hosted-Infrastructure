# Sonarr

## What it's used for

TV show library automation — tracks series, finds new episodes via indexers
(pushed from Prowlarr), and organizes the media library. Runs on the worker
machine.

**Not routed through Cloudflare Tunnel.**
This specific service isn't exposed, as this would be considered an administrative tool, not needing to be accessiable to everyone; but rather its accessiable/hosted on (http://worker-node:8989)

## What was redacted

- Real domain/hostname
- Real media library paths
- API key used for the Prowlarr integration
