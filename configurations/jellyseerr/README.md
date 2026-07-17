# Jellyseerr

## Intended Purpose

Request/management frontend for Jellyfin — lets users browse and request new
media without needing direct access to any admin tools. Runs on the
main machine.

## How it's exposed

- Routed through Cloudflare Tunnel at `requests.example.com`
- Connected internally to Jellyfin via the Docker network (container name, not
  `localhost`)

## What was redacted

- Real domain/hostname
- Real Jellyfin API key (configured inside the Jellyseerr UI, not stored in this repo)
