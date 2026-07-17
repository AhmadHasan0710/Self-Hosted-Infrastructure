# cloudflared (Cloudflare Tunnel)

## Intended Purpose

The central component of the whole setup is the `cloudflared Tunnel.` It establishes an outbound only
connection from each machine/service (both main and worker) to Cloudflare's edge, and routes
incoming requests for each configured hostname to the correct local service;
without ever opening an inbound port or exposing the origin's public IP.

It's also configured so that adding a new service is as simple as: add an ingress rule +
run one `route dns` command, and the DNS record + public route both exist
automatically; no manual DNS management per service.

## What was redacted

- Real tunnel ID and credentials file path
- Real domain name (replaced with `example.com`)
- Any internal port numbers that differ from each service's documented default

## Related documentation

See [`02-cloudflare-tunnel-setup.md`](/Self-Hosted-Infrastructure/documentation/02-cloudflare-tunnel-setup.md) for the full step-by-step.
