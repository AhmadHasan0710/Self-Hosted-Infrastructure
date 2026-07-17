# 05 — Maintenance & Monitoring

## Purpose

Describes the ongoing routine used to keep the environment healthy: monitoring,
backups, and update cadence — the operational side that separates a one-time setup
from a real, maintained production-style environment.

## Monitoring

- **Uptime Kuma** is the primary monitoring tool, running on the main machine
  - A monitor is configured for every public-facing service (hitting the public
    hostname, confirming the tunnel + service + Cloudflare path all work end-to-end)
  - Notification channel configured (e.g. Discord/email webhook) so downtime alerts
    arrive without needing to check the dashboard manually
- **cloudflared** logs are checked periodically for tunnel connection stability
- Cloudflare's dashboard analytics reviewed for traffic anomalies (sudden spikes,
  unexpected countries/IPs hitting Access-protected services)

## Backups

- **Nextcloud** data volume and database backed up on a scheduled job, stored
  separately from the host itself
- **Config volumes** for Jellyfin/Jellyseerr/Sonarr/Radarr/Prowlarr backed up
  periodically so service state (libraries, indexers, settings) isn't lost on
  container rebuild
- Backups tested periodically by restoring into a scratch environment — an untested
  backup is treated as not actually a backup

## Update Cadence

- Docker images updated on a regular schedule rather than immediately on release, to
  avoid breaking changes taking down a service unexpectedly
- `cloudflared` and Tailscale clients kept current since they're the security-critical
  connective tissue of the whole setup
- Host OS patched on a regular schedule (security updates applied promptly, feature
  updates on a slower cadence)

## Routine Checklist (example cadence)

| Task | Frequency |
|---|---|
| Check Uptime Kuma dashboard | Daily (passive, via alerts) |
| Review Cloudflare traffic/analytics | Weekly |
| Update container images | Bi-weekly / monthly |
| Test a backup restore | Monthly |
| Review Tailscale ACLs and connected devices | Monthly |
| Full security review (see doc 06) | Quarterly |
