# 01 — Domain Purchase & DNS Migration to Cloudflare

## Purpose

Documents how the domain was purchased through Hostinger and migrated over to
Cloudflare for DNS management, WAF, and proxying — while keeping registration
itself with Hostinger.

## Prerequisites

- Domain purchased through Hostinger (or any registrar)
- Free Cloudflare account

## Step-by-Step

1. **Add the site to Cloudflare**
   - In the Cloudflare dashboard: Add a Site -> enter the domain -> select plan (Free is sufficient)
   - Cloudflare scans existing DNS records automatically and imports what it finds

2. **Review imported DNS records**
   - Confirm A/CNAME records match what's expected (root domain, `www`, any existing subdomains)
   - Remove any stale/unused records from the old host

3. **Update nameservers at the registrar**
   - In Hostinger's domain management panel, replace the default nameservers with the
     two Cloudflare-assigned nameservers
   - Registration stays with Hostinger; DNS resolution authority moves to Cloudflare

4. **Wait for propagation**
   - Cloudflare will show "Active" once it detects the nameserver change
     (typically anywhere from a few minutes to 24 hours)

5. **Set default proxy behavior**
   - Root domain and subdomains that will be tunnel-routed are set to "Proxied" (orange cloud)
   - This hides the origin IP and lets Cloudflare's edge (WAF, caching, TLS) sit in front

## Verification

- `dig NS yourdomain.com` should return Cloudflare nameservers
- Cloudflare dashboard shows the zone status as **Active**
- SSL/TLS mode set to **Full (strict)** once origin certs are in place (see doc 06)

## Maintenance

- Revisit DNS records any time a new service/subdomain is added (in this setup, most
  of this is automated by the Cloudflare Tunnel — see doc 02)
- Periodically audit DNS records for anything stale or unused

## Troubleshooting

- **Site stuck on "Pending Nameserver Update":** double-check the exact nameserver
  values were entered at the registrar with no typos, and that old NS records were fully replaced, not appended
- **SSL errors after migration:** make sure the encryption mode isn't set to
  "Flexible" if the origin doesn't accept plain HTTP — use Full/Full (strict)
