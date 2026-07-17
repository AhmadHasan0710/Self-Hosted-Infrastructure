# 02 — Cloudflare Tunnel Setup (cloudflared)

## Purpose

Covers installing and configuring `cloudflared` so every self-hosted service is
reachable through the domain/subdomains without ever exposing the origin's public
IP, and so new services automatically get their DNS record + ingress route created.

## Prerequisites

- Domain already migrated to Cloudflare (see doc 01)
- Cloudflare account with Zero Trust enabled (free tier covers this use case)
- A machine to run `cloudflared` on (this setup runs it on the main machine, with a
  second tunnel/connector for the worker machine's services)

## Step-by-Step

1. **Install cloudflared**
   - Installed via the official Cloudflare package repository for the host OS
   - Verified version with `cloudflared --version`

2. **Authenticate**
   - `cloudflared tunnel login` — opens a browser flow to authorize the tunnel
     against the Cloudflare account/zone

3. **Create the tunnel**
   - `cloudflared tunnel create <tunnel-name>`
   - This generates a tunnel ID and credentials file (kept out of version control —
     see `configurations/cloudflared/README.md` for what's redacted)

4. **Define ingress rules**
   - A single `config.yml` maps each subdomain (hostname) to a local service
     (e.g. `jellyfin.example.com` -> `http://localhost:8096`)
   - See `configurations/cloudflared/config.yml.example` for the redacted template

5. **Enable automatic DNS record creation**
   - Running `cloudflared tunnel route dns <tunnel-name> <hostname>` for each service
     automatically creates the matching CNAME record pointed at the tunnel — no manual
     DNS entry needed
   - New services just need a new ingress line + one `route dns` command and the
     public route + DNS record both exist

6. **Run as a service**
   - `cloudflared service install` so the tunnel runs persistently and restarts with
     the machine
   - Repeated on the worker machine for its services (Prowlarr, Sonarr, Radarr),
     either as a second tunnel or additional ingress rules on the same tunnel
     depending on network reachability between machines
   - Note: qBittorrent also runs on the worker machine but intentionally has **no**
     ingress rule at all — it's excluded from the tunnel on purpose (see doc 06 and
     `configurations/qbittorrent/README.md`)

## Verification

- `cloudflared tunnel list` shows the tunnel as healthy/connected
- Visiting each subdomain from an external network resolves to the correct service
- `dig subdomain.example.com` shows a CNAME pointing to `<tunnel-id>.cfargotunnel.com`
  rather than any real IP

## Maintenance

- When adding a new service: add an ingress rule + run the `route dns` command — no
  other DNS changes needed
- Periodically check `cloudflared` logs for connection health
- Rotate tunnel credentials if the machine is rebuilt or credentials are suspected compromised

## Troubleshooting

- **502 from a subdomain:** local service isn't running or is bound to the wrong
  port/interface — check ingress rule matches the actual local address
- **Tunnel shows disconnected:** check outbound connectivity from the host (tunnel is
  outbound-only, so this is usually a local network/firewall issue, not Cloudflare's side)
