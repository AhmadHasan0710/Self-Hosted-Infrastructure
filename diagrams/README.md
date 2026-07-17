# Diagrams

## [Cloudflare & Network Architecture](./Cloudflared&NetworkArchitectureDiagram.jpeg)

- Shows how traffic reaches the server and how it's locked down before touching
  any service.
- All inbound traffic is blocked at the firewall by default — only VPN and LAN
  connections are allowed directly, no ports opened to the public internet.
- Everything else routes through the Cloudflare Tunnel (`cloudflared`), which
  makes an outbound only connection to Cloudflare's edge and forwards traffic
  to the correct local service by hostname.
- Cloudflare Zero Trust sits in front of the private services (Jellyfin,
  Jellyseerr, Nextcloud, Uptime Kuma), so only authenticated, approved users
  can reach them.

### What was redacted

- Real hostnames and internal port numbers
- Real domain name (replaced with `example.com`)
- Zero Trust policy names and allowed user/email list

### Related documentation

See [`02-cloudflare-tunnel-setup.md`](../documentation/02-cloudflare-tunnel-setup.md) for the full step-by-step.

---

## [Media Request Pipeline](./MediaRequestPipeline.jpeg)

- Walks through what happens when a user requests a show or movie, split
  across two machines.
- The main server runs Jellyseerr, Jellyfin, the VPN, and `cloudflared`.
- The worker PC runs qBittorrent, Prowlarr, Sonarr, and Radarr, kept separate
  so downloading and indexing don't congest the main server.
- A request in Jellyseerr goes to Sonarr/Radarr, which checks Prowlarr's
  indexers.
- No match ends the flow with an error; a match is sent to qBittorrent,
  imported into Jellyfin once downloaded.
- Jellyseerr notifies the user it's ready. The whole process usually takes
  a few minutes to about 30 minutes.

### What was redacted

- Real indexer names configured in Prowlarr
- Internal hostnames/IPs between the main server and worker PC
- API keys or arr-stack connection details

### Related documentation

See [`03-service-deployment.md`](../documentation/03-service-deployment.md) for the full step-by-step.