# Documentation

Step-by-step procedures covering how this entire environment was built and how it
continues to be maintained. Each file is written so someone with general
networking/system administration knowledge could follow it and reproduce the setup.

## Files in this folder

| File                               | Specifics |

| `01-domain-and-dns-setup.md`       | Domain Management, migrating DNS to Cloudflare |
| `02-cloudflare-tunnel-setup.md`    | Installing/configuring `cloudflared`, automated route + DNS creation |
| `03-service-deployment.md`         | Deploying each service on the main and worker machines |
| `04-tailscale-setup.md`            | Setting up the private VPN mesh between machines |
| `05-maintenance-and-monitoring.md` | Ongoing maintenance routine, backups, Uptime Kuma monitoring |
| `06-security-hardening.md`         | Access control, WAF rules, secrets handling, patching cadence |

