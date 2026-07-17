# Nextcloud

## What it's used for

Private cloud file storage; replaces both Dropbox & Google Drive; mainly for personal use. Runs on the main machine.

## How it's exposed

- Routed through Cloudflare Tunnel at `nextcloud.example.com`
- Placed behind a Cloudflare Access policy in addition to Nextcloud's own login,
  since it holds personal files (defense in depth)

## What was redacted

- Real domain/hostname
- Database credentials / environment secrets
- Real storage volume paths
