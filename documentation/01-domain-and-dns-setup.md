# 01 — Domain Purchase & DNS Migration to Cloudflare

## Purpose

This document covers the first stage of the project: getting a domain purchased
and DNS pointed at Cloudflare so every later step (the tunnel, the services, and
the security layers) has a stable foundation to build on.

The domain could have been purchased directly through Cloudflare's registrar,
but Hostinger was offering a better deal at the time, so the domain was
purchased there instead. Rather than transferring the registration itself, the
domain was migrated over to Cloudflare purely for DNS management by updating
the nameservers, while registration stayed with Hostinger.

This was done using a **free Cloudflare account**, which is enough to cover
this setup entirely. The free tier includes DNS management, proxying, and the
security features used throughout this project, along with Cloudflare Access
support for up to fifty authenticated users, more than enough for a personal
environment like this one.



## Prerequisites

- **Domain** purchased through Hostinger (or any registrar)
- **Free Cloudflare account**



## Step-by-Step

1. **Add the site to Cloudflare** — In the Cloudflare dashboard, choose Add a
   Site, enter the domain, and select the Free plan. Cloudflare scans for
   existing DNS records automatically and imports whatever it finds.

2. **Review the imported DNS records** — Confirm the A and CNAME records match
   what's expected, including the root domain, `www`, and any existing
   subdomains, then remove anything stale left over from the previous host.

3. **Update the nameservers at Hostinger** — In Hostinger's domain management
   panel, the default nameservers were replaced with the two nameservers
   Cloudflare assigned. Registration remains with Hostinger, but DNS
   resolution authority moves fully to Cloudflare.

4. **Wait for propagation** — Cloudflare marks the zone as Active once it
   detects the nameserver change, which can take anywhere from a few minutes
   to about 24 hours.

5. **Set the default proxy behavior** — The root domain and any subdomain
   that would later be routed through the tunnel are set to Proxied (the
   orange cloud icon). This keeps the origin IP hidden and puts Cloudflare's
   edge, including the WAF and TLS termination, in front of everything.



## Verification

- **`dig NS yourdomain.com`** returns Cloudflare's nameservers
- **Cloudflare dashboard** shows the zone status as Active
- **SSL/TLS mode** is set to Full (strict) once origin certificates are in
  place, see [`06-security-hardening.md`](./06-security-hardening.md)



## Maintenance

- DNS records are revisited any time a new service or subdomain is added,
  though in this setup most of that is automated once the Cloudflare Tunnel is
  in place, see [`02-cloudflare-tunnel-setup.md`](./02-cloudflare-tunnel-setup.md)
- DNS records are periodically audited for anything stale or unused



## Troubleshooting

- **Site stuck on Pending Nameserver Update** — double check the exact
  nameserver values were entered at the registrar with no typos, and that the
  old nameservers were fully replaced rather than appended to.
- **SSL errors after migration** — make sure the encryption mode isn't set to
  Flexible if the origin doesn't accept plain HTTP; use Full or Full (strict)
  instead.
