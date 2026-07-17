# Documentation

Step-by-step procedures covering how this entire environment was built and how it
continues to be maintained. Each file is written so someone with general
networking/system administration knowledge could follow it and reproduce the
setup.

## Files in this Folder

| File                                                                          | Specifics                                                                       |
|--------------------------------------------------------------------------------|-----------------------------------------------------------------------------------|
| [`01-domain-and-dns-setup.md`](./01-domain-and-dns-setup.md)                 | Purchasing the domain, migrating DNS management from Hostinger to Cloudflare      |
| [`02-cloudflare-tunnel-setup.md`](./02-cloudflare-tunnel-setup.md)           | Installing/configuring `cloudflared`, ingress rules, and automated route + DNS creation |
| [`03-service-deployment.md`](./03-service-deployment.md)                     | Deploying each service via Docker on the main and worker machines                 |
| [`04-tailscale-setup.md`](./04-tailscale-setup.md)                           | Setting up the private VPN mesh between the main and worker machines              |
| [`05-maintenance-and-monitoring.md`](./05-maintenance-and-monitoring.md)     | Ongoing maintenance routine, backups, and Uptime Kuma monitoring configuration     |
| [`06-security-hardening.md`](./06-security-hardening.md)                     | Access control, Cloudflare Access Policies, secrets handling, and etc.     |