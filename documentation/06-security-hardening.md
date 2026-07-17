# 06 — Security Hardening

## Purpose

Documents the security controls layered on top of the base setup — this is the part
that most directly demonstrates security engineering skill, as opposed to just
"getting it working."

## Layers of Control

1. **No exposed origin**
   - Cloudflare Tunnel means the origin machine never has an inbound port open to the
     public internet for these services — there is no IP to attack directly
2. **Cloudflare WAF / Firewall Rules**
   - Rules configured to block obviously malicious patterns (known bad user agents,
     suspicious countries if not relevant to the intended audience, rate limiting on
     login endpoints)
3. **Cloudflare Access**
   - Sensitive services (e.g. Nextcloud, Uptime Kuma dashboard, the `*arr` stack) are
     placed behind Cloudflare Access policies requiring authentication (e.g. one-time
     PIN via email, or an identity provider) before the request even reaches the tunnel
4. **Least-privilege networking**
   - Services only talk to each other over an internal Docker network; nothing binds
     to a public interface unless explicitly intended to
5. **Tailscale for admin-only access**
   - True internal/admin interfaces (SSH, container management, direct DB access) are
     never exposed via Cloudflare at all — only reachable over the private Tailscale
     mesh with its own ACLs
6. **Secrets management**
   - API tokens, tunnel credentials, and `.env` files are excluded from version
     control (`.gitignore`) and only redacted example files are committed to this repo
7. **Patching cadence**
   - See doc 05 — the security posture depends on cloudflared/Tailscale/images being
     kept current

## Verification

- Attempted access to the origin IP directly (where known) fails/times out — traffic
  only succeeds through the Cloudflare-fronted hostname
- Access-protected services correctly prompt for authentication before reaching the
  application
- Reviewed Cloudflare Access logs to confirm policy enforcement is actually happening,
  not just configured

## Maintenance

- Quarterly review of WAF/Access rules for anything overly permissive or stale
- Rotate any long-lived tokens (Cloudflare API tokens, tunnel credentials) periodically
- Review Tailscale ACLs alongside Cloudflare Access policies to make sure the two
  layers are still consistent with each other as services are added/removed

## What NOT to include in this repo

- Real domain name tied to personal identity (use a placeholder if the write-up needs
  a concrete example)
- Tunnel IDs, zone IDs, API tokens, or credentials files
- Internal IP ranges or real hostnames
- Anything that would let someone map out exactly what's running where
