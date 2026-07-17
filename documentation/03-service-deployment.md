# 03 — Service Deployment

## Purpose

Covers how each hosted service was deployed on the main machine and the worker
machine, and how each one is wired into the Cloudflare Tunnel.

## Prerequisites

- Cloudflare Tunnel running (see doc 02)
- Docker + Docker Compose installed on both the main machine and the worker machine
  (all services in this setup run as containers for consistency and easy backups)

## Machine Layout

| Machine | Services |
|---|---|
| Main | Website, Jellyfin, Jellyseerr, Nextcloud, Uptime Kuma |
| Worker | Prowlarr, Sonarr, Radarr, qBittorrent |

## Step-by-Step (general pattern used for every service)

1. **Define the service in Docker Compose**
   - Each service gets its own compose file (or a section in a shared one) with a
     pinned image version, persistent volume mapping, and an internal port
2. **Bind to localhost / internal network only**
   - No service ports are published to the public interface — everything is only
     reachable via the internal Docker network or localhost, which the tunnel then
     reaches over its outbound connection
3. **Add an ingress rule in `cloudflared`'s config**
   - Maps `service.example.com` -> `http://localhost:<port>`
4. **Run `route dns` for the new hostname**
   - Creates the DNS record automatically (see doc 02)
5. **Apply service-specific hardening**
   - Reverse proxy headers, auth requirements, and any Cloudflare Access policies
     per service (see doc 06 and each service's config README)

## Per-Service Notes

- **Website** — static site (or lightweight app) served on the root/main domain
- **Jellyfin** — media server; hardware transcoding passthrough configured if
  applicable; library folders mounted read-only where possible
- **Jellyseerr** — connected to Jellyfin as its backing media server; used purely as
  a request/management front end
- **Nextcloud** — paired with a database container; storage volume backed up
  separately (see doc 05)
- **Uptime Kuma** — configured with a monitor for every other service (public
  hostname + internal port), used as the single pane of glass for uptime status
- **Prowlarr / Sonarr / Radarr** — run together on the worker machine, networked so
  Prowlarr can push indexers to Sonarr/Radarr internally; only their web UIs are
  exposed through the tunnel, not any download traffic
- **qBittorrent** — the actual download client Sonarr/Radarr hand grabs off to; runs
  on the worker machine but is **deliberately not routed through the Cloudflare
  Tunnel at all** — no public hostname, no Access policy, nothing. It's reachable
  only over Tailscale or from `localhost` on the worker machine. This is a
  conscious security tradeoff, not an inconsistency: a torrent client's web UI is a
  much higher-value target than the rest of the stack, so it gets no public attack
  surface whatsoever

## Verification

- Each subdomain loads the correct service from outside the local network
- Uptime Kuma shows all monitors green
- Inter-service integrations (Jellyseerr <-> Jellyfin, Prowlarr <-> Sonarr/Radarr)
  tested end-to-end after deployment

## Maintenance

- Image updates applied on a regular cadence (see doc 05)
- Compose files and `.env` files backed up (secrets excluded from any public repo)

## Troubleshooting

- **Service unreachable via subdomain but works on localhost:** check the ingress
  rule hostname/port match and that `cloudflared` was restarted after config changes
- **Inter-service integration fails:** confirm both containers are on the same
  Docker network and referencing each other by container name, not `localhost`
