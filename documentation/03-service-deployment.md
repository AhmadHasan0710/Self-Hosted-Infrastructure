# Service Deployment

## Overall

This document covers how each service was deployed, how it runs locally on its
own machine, and how the services meant to be public are wired into the
Cloudflare Tunnel through the ingress file.


## Prerequisites

- **Cloudflare Tunnel** running, see [`02-cloudflare-tunnel-setup.md`](./02-cloudflare-tunnel-setup.md)
- **Docker and Docker Compose** installed on both the main machine and the
  worker machine; every self-hosted service in this setup runs as a container
  for consistency and easy backups


## Machine Layout

| Machine | Services |
|---|---|
| Main | Jellyfin, Jellyseerr, Nextcloud, Uptime Kuma |
| Worker | Prowlarr, Sonarr, Radarr, qBittorrent |

The personal/portfolio site is not part of this Docker layout at all. It's
deployed on **Vercel** rather than self-hosted, with `example.com` pointed at
Vercel through a standard Cloudflare DNS record, with the ingress rule setup as a backup;
in any instance Vercel goes down.



## General Deployment Pattern

Every self-hosted service follows the same base pattern:

1. **Defined in Docker Compose** — with a pinned image version, persistent
   volume mapping, and an internal port.
2. **Bound internally only** — no service publishes a port to the public
   interface. Everything is only reachable over the VPN or
   localhost, which the tunnel then reaches over its own outbound connection.
3. **Given an ingress rule** — services meant to be public get a matching
   ingress rule in `cloudflared`'s config, mapping their hostname to
   `http://localhost:<port>`.
4. **Registered with `route dns`** — creating the DNS record automatically, as
   covered in [`02-cloudflare-tunnel-setup.md`](./02-cloudflare-tunnel-setup.md).
5. **Hardened where needed** — including Access policies for select
   applications, covered in [`06-security-hardening.md`](./06-security-hardening.md).


## Per-Service Notes

- **Jellyfin** — runs on the main machine as the media server for the whole
  setup, with library folders mounted so both it and the automation stack can
  reach the same files.

- **Jellyseerr** — runs on the main machine and connects to Sonarr and Radarr
  by IP address rather than container name, since those two services live on
  the worker machine rather than the same host.

- **Nextcloud** — runs on the main machine alongside its own database
  container, using standard, well documented configuration for private file
  storage and sync.

- **Uptime Kuma** — runs on the main machine and monitors every other service
  by either its local IP address or the IP address of the worker machine,
  depending on where that service lives. It sends notifications through its
  own SMTP server connection, emailing an alert whenever a monitored service
  goes down or comes back up. Expanded on in
  [`05-maintenance-and-monitoring.md`](./05-maintenance-and-monitoring.md).

- **Prowlarr** — runs on the worker machine and uses an indexer proxy to reach
  certain torrent sites, then pushes those indexers down to Sonarr and Radarr
  so they can search across the same sources.

- **Sonarr and Radarr** — run on the worker machine and are connected to the
  main machine's NFS file share, which gives them access to Jellyfin's movie
  and show library folders so completed downloads land exactly where Jellyfin
  expects them.

- **qBittorrent** — runs on the worker machine and is the client Sonarr and
  Radarr hand completed matches off to.

  A few specific settings were tuned here to keep the connection reliable
  and fast:
  - Torrents are limited to downloading one at a time instead of all at once
  - The client is port forwarded manually through the router instead of
    relying on default settings
  - Only specific file types are allowed through so bandwidth isn't wasted
    on unwanted files

  qBittorrent, alongside all other services maintained on the worker server, are deliberately, **not** routed through the Cloudflare
  Tunnel at all:
  - No public hostname and no Access policy
  - Reachable only over Tailscale or from localhost on the worker machine
  - Covered further in [`04-tailscale-setup.md`](./04-tailscale-setup.md)
    and [`06-security-hardening.md`](./06-security-hardening.md)

Every service that is exposed publicly sits behind Cloudflare, which not only
hosts the traffic but also runs Zero Trust in front of select applications so
only authenticated users can reach them. The remaining services not called out
above run on their documented default configuration without any notable
customization.


## Verification

- **Each subdomain** loads the correct service from outside the local network
- **Uptime Kuma** shows every monitor green
- **Cross-service integrations**, such as Jellyseerr to Jellyfin and Prowlarr
  to Sonarr/Radarr, were tested end to end after deployment

---

## Maintenance

- Image updates are applied on a regular cadence, covered in
  [`05-maintenance-and-monitoring.md`](./05-maintenance-and-monitoring.md)
- Compose files and `.env` files are backed up, with secrets excluded from any
  public repo


## Troubleshooting

- **Service unreachable via subdomain but works on localhost** — check that
  the ingress rule's hostname and port match, and that `cloudflared` was
  restarted after the config change.
- **Cross-service integration fails** — confirm both services are referencing
  each other by the correct IP address or container name depending on whether
  they're on the same machine, and that any required network path (like the
  NFS share) is actually reachable.
