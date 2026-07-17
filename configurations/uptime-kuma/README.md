# Uptime Kuma

## What it's used for

Uptime/status monitoring dashboard for every other service in this environment.
Runs on the main machine and is the single pane of glass for whether everything
is actually reachable end-to-end (public hostname -> Cloudflare -> tunnel -> service).

## How it's exposed

- Routed through Cloudflare Tunnel at `kuma.example.com`
- Placed behind a Cloudflare Access policy since it exposes operational detail
  about the rest of the environment

## Files in this folder

- `docker-compose.yml.example` — redacted compose definition
- `monitors-list.md` — list of what's monitored and how (referenced, not the raw
  export, since the export can contain notification webhook URLs)

## What was redacted

- Real domain/hostname
- Notification webhook URLs (Discord/email/etc.)
