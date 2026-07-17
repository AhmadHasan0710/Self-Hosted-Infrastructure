# Jellyfin (Media Server)

## Intended Purpose

Self-hosted media streaming server hosting all available shows/movies to all authenticaed users.

## How it's exposed

- Routed through Cloudflare Tunnel at `jellyfin.example.com`
- Set behind a Cloudflare Zero Trust Access policy (Jellyfin also has their own access policies),
- Only authenticated Users can access the site

## What was redacted

- Real media library paths
- Real domain/hostname
