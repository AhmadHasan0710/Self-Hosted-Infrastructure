# Tailscale Private VPN Setup

## Overall

This document covers setting up Tailscale as a private mesh network for
internal only access, reaching services that aren't and shouldn't be exposed
publicly through Cloudflare, such as qBittorrent's web UI.

Tailscale runs on the main server and does not require any port forwarding on
the main router at all, unlike a traditional self-hosted VPN. Only devices
that have been explicitly authenticated to the tailnet are allowed to join,
and from there only the specific network or subnet that's been advertised is
reachable, rather than opening broad access to the whole home network.

In this setup, the main server advertises the subnet **`192.168.254.0/24`**
and is also configured as an **exit node**. Any device connecting through
Tailscale gets access to the services that live on that subnet, including
local only services like Prowlarr, without needing a separate connection or
any router configuration on the network those devices are physically on.


## Prerequisites

- **Free Tailscale account**
- **Tailscale installed** on the main machine, the worker machine, and any
  admin devices (laptop or phone) that need private access


## Step-by-Step

1. **Install Tailscale on each device** — Installed via the official package
   for each operating system.

2. **Authenticate each device to the tailnet** — `tailscale up` on each
   machine, authorized through the Tailscale admin console.

3. **Assign stable device names** — Devices were renamed in the Tailscale
   admin console for clarity, such as `main-server` and `worker-node`, instead
   of relying on default hostnames.

4. **Advertise the subnet and exit node** — The main server advertises
   `192.168.254.0/24` as a routable subnet and is also flagged as an exit
   node, so authenticated devices can reach local only services on that
   network through it.

5. **Configure ACLs** — Tailscale's ACL policy file restricts which devices
   can reach which others. For example, an admin laptop can reach everything,
   while the worker node only needs to reach the main machine for internal
   integrations, not the other way around.

6. **Use MagicDNS** — Enabled so devices can be reached by their tailnet name
   instead of remembering Tailscale IP addresses.


## Verification

- **`tailscale status`** shows all expected devices connected
- **Internal only interfaces**, such as direct SSH access or container
  management UIs, are reachable from an authorized device over the tailnet,
  but not from the public internet at all
- **A device connecting through the exit node** can reach local only services
  like Prowlarr on the `192.168.254.0/24` subnet


## Maintenance

- The ACL policy is reviewed periodically as new devices or services are added
- Old devices no longer in use are removed or de-authorized from the tailnet


## Troubleshooting

- **Device shows in the tailnet but is unreachable** — check that ACL rules
  aren't unintentionally blocking it, and that the Tailscale service is
  actually running on that device.
- **MagicDNS not resolving** — confirm it's enabled at the tailnet level in
  the admin console, not just on the individual device.
