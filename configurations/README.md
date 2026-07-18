# Configurations

This folder holds a subfolder per service, each containing:

- A `README.md` explaining what the service is used for, how it's wired into the
  Cloudflare Tunnel, and any service-specific notes
- Redacted/template config files (`-config`) showing the real structure used,
  with secrets, tokens, IDs, and internal hostnames

## Services

| Folder          | Service     | Machine | Purpose                                                                                              |
|------------------|-------------|---------|--------------------------------------------------------------------------------------------------------|
| `cloudflared/`   | Tunnel      | Main    | Routes all public traffic to internal services without exposing origin IP; auto-creates DNS records   |
| `website/`       | Website     | Main    | Hosted on the root/main domain                                                                        |
| `jellyfin/`      | Jellyfin    | Main    | Self-hosted media streaming server                                                                    |
| `jellyseerr/`    | Jellyseerr  | Main    | Media request front-end for Jellyfin                                                                  |
| `nextcloud/`     | Nextcloud   | Main    | Private cloud file storage/sync                                                                       |
| `uptime-kuma/`   | Uptime Kuma | Main    | Uptime/status monitoring dashboard for every service                                                  |
| `prowlarr/`      | Prowlarr    | Worker  | Central indexer manager for the automation stack                                                      |
| `sonarr/`        | Sonarr      | Worker  | TV show library automation                                                                             |
| `radarr/`        | Radarr      | Worker  | Movie library automation                                                                               |
| `qbittorrent/`   | qBittorrent | Worker  | Download client for the automation stack — **not** routed through the Cloudflare Tunnel, Tailscale-only |
| `tailscale/`     | Tailscale   | Both    | Private VPN mesh for internal/admin-only access                                                        |

## Convention Used in Each Config File

Each `-config` file mirrors the real config's structure exactly, with sensitive
values replaced like this:

```yaml
tunnel: <TUNNEL_ID>
credentials-file: <PATH_TO_CREDENTIALS_JSON>
```

This keeps the templates genuinely useful/reproducible for anyone reading the
repo, without leaking anything about the live environment.
