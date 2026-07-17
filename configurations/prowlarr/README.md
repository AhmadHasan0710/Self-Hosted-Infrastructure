# Prowlarr

## What it's used for

Central indexer manager for the media automation setup; configures indexers once,
then pushes them out to Sonarr and Radarr; rather than configuring each app
separately. Runs on the worker machine.

**Not routed through Cloudflare Tunnel.**
This specific service isn't exposed, as this would be considered an administrative tool, not needing to be accessiable to everyone; but rather its accessiable/hosted on (http://worker-node:9696) or authenticated users via Tailscale

## What was redacted

- Real domain/hostname
- API keys used for the Sonarr/Radarr integration (configured within the Prowlarr UI)
