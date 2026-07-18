# Maintenance & Monitoring

## Overall

This document describes the ongoing routine used to keep the environment
healthy: monitoring, backups, and update cadence. This is the operational side
that separates a one time setup from a real, maintained environment.


## Monitoring

**Uptime Kuma** is the primary monitoring tool and runs on the main machine.
A monitor is configured for every service, checking either the service's
local IP address or the worker machine's IP address depending on where that
service lives, and for public facing services the public hostname is checked
as well, confirming the tunnel, the service, and Cloudflare are all working
end to end together.

Uptime Kuma sends notifications through its own **SMTP server connection**,
emailing an alert as soon as any monitored service goes down or comes back
up, so downtime is caught without needing to check the dashboard manually.

`cloudflared` logs are checked periodically for tunnel connection stability,
and Cloudflare's dashboard analytics are reviewed for traffic anomalies, such
as sudden spikes or unexpected countries and IPs hitting Access protected
services.


## Backups

**Nextcloud's** data volume and database are backed up on a scheduled job,
stored separately from the host itself. **Config volumes** for Jellyfin,
Jellyseerr, Sonarr, Radarr, and Prowlarr are backed up periodically as well,
so service state such as libraries, indexers, and settings isn't lost if a
container needs to be rebuilt.


## Update Cadence

Docker images are updated on a regular schedule rather than immediately on
release, to avoid a breaking change taking a service down unexpectedly.
`cloudflared` and the Tailscale clients are kept current since they're the
security critical connective tissue of the whole setup, covered further in
[`06-security-hardening.md`](./06-security-hardening.md). The host OS is
patched on a regular schedule, with security updates applied promptly and
feature updates on a slower cadence.


## Routine Checklist

| Task | Frequency |
|---|---|
| Check Uptime Kuma dashboard | Daily (passive, via alerts) |
| Review Cloudflare traffic and analytics | Weekly |
| Update container images | Bi-weekly / monthly |
| Test a backup restore | Monthly |
| Review Tailscale ACLs and connected devices | Monthly |
| Full security review, see [`06-security-hardening.md`](./06-security-hardening.md) | Quarterly |
