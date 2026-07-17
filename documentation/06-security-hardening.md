# 06 — Security Hardening

## Purpose

This document covers the security controls layered on top of the base setup.
This is the part that most directly demonstrates security engineering skill,
rather than just getting everything working.


## Layers of Control

1. **No exposed origin** — The Cloudflare Tunnel means the origin machine
   never has an inbound port open to the public internet for these services.
   There is no IP address to attack directly, covered in
   [`02-cloudflare-tunnel-setup.md`](./02-cloudflare-tunnel-setup.md).

2. **Bot detection, DDoS protection, and full TLS encryption** — Bot Fight
   Mode challenges automated bot traffic before it reaches any service.
   Cloudflare's network level DDoS protection absorbs volumetric attacks at
   the edge. Full (strict) TLS encrypts traffic on both the client side and
   the server side, rather than dropping to plain HTTP internally. All three
   are configured at the tunnel layer, detailed in
   [`02-cloudflare-tunnel-setup.md`](./02-cloudflare-tunnel-setup.md).

3. **Cloudflare Access (Zero Trust)** — Sensitive services, including
   Nextcloud, the Uptime Kuma dashboard, and the `*arr` stack, are placed
   behind Cloudflare Access policies requiring authentication, such as a one
   time PIN via email or an identity provider, before the request is even
   allowed to reach the tunnel.

4. **Least privilege networking** — Services only talk to each other over an
   internal Docker network, and nothing binds to a public interface unless
   it's explicitly meant to. Sonarr and Radarr reach Jellyfin's media library
   only through the main machine's NFS share rather than any broader network
   access, covered in [`03-service-deployment.md`](./03-service-deployment.md).

5. **Tailscale for admin only access** — True internal and admin interfaces,
   such as SSH, container management, and direct database access, are never
   exposed through Cloudflare at all. They are only reachable over the
   private Tailscale mesh, with its own ACLs and an advertised subnet limited
   to `192.168.254.0/24`, covered in [`04-tailscale-setup.md`](./04-tailscale-setup.md).
   qBittorrent follows the same principle: it has no public route whatsoever
   and is reachable only over Tailscale, since a torrent client's web UI is a
   much higher value target than the rest of the stack.

6. **Secrets management** — API tokens, tunnel credentials, and `.env` files
   are excluded from version control through `.gitignore`, with only redacted
   example files committed to this repository.

7. **Patching cadence** — The overall security posture depends on
   `cloudflared`, Tailscale, and every container image being kept current,
   covered in [`05-maintenance-and-monitoring.md`](./05-maintenance-and-monitoring.md).


## Verification

- **Direct origin access** — attempting to reach the origin IP directly,
  where known, fails or times out; traffic only succeeds through the
  Cloudflare fronted hostname
- **Access protected services** — correctly prompt for authentication before
  reaching the application
- **Cloudflare Access logs** — reviewed to confirm policy enforcement is
  actually happening, not just configured


## Maintenance

- WAF and Access rules are reviewed quarterly for anything overly permissive
  or stale
- Long lived tokens, including Cloudflare API tokens and tunnel credentials,
  are rotated periodically
- Tailscale ACLs are reviewed alongside Cloudflare Access policies to make
  sure the two layers stay consistent with each other as services are added
  or removed


## What NOT to Include in This Repo

- The real domain name tied to personal identity, replaced with a placeholder
  wherever a concrete example is needed
- Tunnel IDs, zone IDs, API tokens, or credentials files
- Internal IP ranges or real hostnames
- Anything that would let someone map out exactly what's running where
