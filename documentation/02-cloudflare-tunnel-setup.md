# 02 — Cloudflare Tunnel Setup (cloudflared)

## Overall

This document covers creating the Cloudflare Tunnel and connecting every locally
hosted service to it, so each service is securely reachable on the public domain
without ever opening an inbound port on the server itself.



## Prerequisites

- **Domain** already migrated to Cloudflare, see [`01-domain-and-dns-setup.md`](./01-domain-and-dns-setup.md)
- **Cloudflare account** with Zero Trust enabled (the free tier covers this use case)
- **A machine** to run `cloudflared` on (this setup runs it on the main machine)


## Step-by-Step

1. **Install cloudflared** — Installed via the official Cloudflare package
   repository for the host OS, and confirmed with `cloudflared --version`.

2. **Authenticate** — Running `cloudflared tunnel login` opens a browser flow
   to authorize the tunnel against the Cloudflare account and zone.

3. **Create the tunnel** — `cloudflared tunnel create <tunnel-name>` generates
   a tunnel ID and a credentials file. Both are kept out of version control;
   see [`configurations/cloudflared/README.md`](../configurations/cloudflared/README.md)
   for what's redacted.

4. **Define the ingress rules** — A single `config.yml` maps each hostname to
   the local service behind it, for example `jellyfin.example.com` to
   `http://localhost:8096`. This ingress file is what connects every locally
   running service to the public domain. See
   [`configurations/cloudflared/cloudflared-config.md`](../configurations/cloudflared/cloudflared-config.md)
   for the redacted template.

5. **Enable automatic DNS record creation** — Running
   `cloudflared tunnel route dns <tunnel-name> <hostname>` for each service
   automatically creates the matching DNS record pointed at the tunnel, with
   no manual DNS entry required. Adding a new service going forward only needs
   a new ingress line plus one `route dns` command, and both the DNS record
   and the public route exist automatically.

6. **Run cloudflared as a persistent service** — `cloudflared service install`
   keeps the tunnel running and restarts it with the machine. The same process
   is repeated on the worker machine for its services (Prowlarr, Sonarr,
   Radarr). qBittorrent also runs on the worker machine but intentionally has
   no ingress rule at all; it's excluded from the tunnel on purpose, see
   [`06-security-hardening.md`](./06-security-hardening.md) and
   [`configurations/qbittorrent/README.md`](../configurations/qbittorrent/README.md).


## Security Hardening Applied at the Tunnel Layer

Beyond simply hiding the origin IP, several Cloudflare features were enabled to
harden the traffic that does reach the tunnel:

- **Bot Fight Mode** — detects and challenges automated bot traffic before it
  reaches any service.
- **DDoS protection** — enabled by default at the network layer through
  Cloudflare's edge, absorbing volumetric attacks before they ever reach the
  origin.
- **Full (strict) TLS encryption** — encrypts traffic on both the client side,
  visitor to Cloudflare's edge, and the server side, Cloudflare's edge to the
  origin, rather than dropping to plain HTTP internally.
- **Cloudflare Access (Zero Trust)** — applied on top of the tunnel for the
  services that shouldn't be publicly open to anyone, requiring authentication
  before a request is even allowed to reach the ingress rule. Expanded on in
  [`06-security-hardening.md`](./06-security-hardening.md).


## Verification

- **`cloudflared tunnel list`** shows the tunnel as healthy and connected
- **Each subdomain** resolves to the correct service when visited from an
  external network
- **`dig subdomain.example.com`** shows a CNAME pointing at
  `<tunnel-id>.cfargotunnel.com` rather than any real IP address


## Maintenance

- Adding a new service only requires an ingress rule plus the `route dns`
  command; no other DNS changes are needed
- `cloudflared` logs are checked periodically for connection health
- Tunnel credentials are rotated if the machine is rebuilt or credentials are
  suspected to be compromised


## Troubleshooting

- **502 from a subdomain** — the local service isn't running or is bound to
  the wrong port or interface. Check that the ingress rule matches the actual
  local address.
- **Tunnel shows disconnected** — check outbound connectivity from the host,
  since the tunnel is outbound only, this is usually a local network or
  firewall issue rather than something on Cloudflare's side.
