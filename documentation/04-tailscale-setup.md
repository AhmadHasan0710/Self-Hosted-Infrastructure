# 04 — Tailscale Private VPN Setup

## Purpose

Covers setting up Tailscale as a private mesh network between the main machine, the
worker machine, and any personal devices — for direct/internal-only access to things
that aren't (and shouldn't be) exposed publicly through Cloudflare.

## Prerequisites

- Free Tailscale account
- Tailscale installed on: main machine, worker machine, and any admin devices
  (laptop/phone) that need private access

## Step-by-Step

1. **Install Tailscale on each device**
   - Installed via the official package for each OS
2. **Authenticate each device to the tailnet**
   - `tailscale up` on each machine, authorized through the Tailscale admin console
3. **Assign stable device names**
   - Renamed devices in the Tailscale admin console for clarity (e.g. `main-server`,
     `worker-node`) instead of relying on default hostnames
4. **Configure ACLs**
   - Used Tailscale's ACL policy file to restrict which devices can reach which
     others (e.g. admin laptop can reach everything; worker node only needs to reach
     main machine for internal integrations, not the reverse)
5. **Use MagicDNS**
   - Enabled so devices can be reached by their tailnet name rather than remembering
     Tailscale IPs

## Verification

- `tailscale status` shows all expected devices connected
- Can reach internal-only admin interfaces (e.g. direct SSH, container management
  UIs) from an authorized device over the tailnet, but these are not reachable from
  the public internet at all

## Maintenance

- Periodically review the ACL policy as new devices/services are added
- Remove/de-authorize old devices no longer in use

## Troubleshooting

- **Device shows in tailnet but unreachable:** check ACL rules aren't unintentionally
  blocking it, and that the device's Tailscale service is actually running
- **MagicDNS not resolving:** confirm it's enabled at the tailnet level in the admin
  console, not just on the individual device
