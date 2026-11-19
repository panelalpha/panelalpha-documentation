---
title: Fastly
description: Protect WordPress instances with Fastly CDN, edge TLS, cache purge, and Under Attack Mode in PanelAlpha.
nav:
  group: Integrations
  order: 11
---

# Fastly

[Fastly](https://www.fastly.com/) provides an edge CDN in front of WordPress instances managed by PanelAlpha. Traffic reaches the origin only through Fastly, while DNS for customer domains continues to be managed by the DNS provider configured on the plan.

With Fastly enabled on a plan, PanelAlpha can automatically:

- create a Fastly service for the instance
- point the origin backend to the hosting server IP (without publishing that IP in public DNS)
- provision TLS at the edge
- sync Fastly-resolved DNS records (A / AAAA / `www` / ACME validation) to the plan DNS provider
- install a WordPress MU-plugin for cache tags and automatic purge on content updates

Clients keep a simple interface: **Purge Cache** and **Under Attack Mode**.

## Prerequisites

- A Fastly account with an [API token](https://www.fastly.com/documentation/guides/account-info/account-management/using-api-tokens/)
- A DNS provider configured on the plan (for example Cloudflare or PowerDNS) so PanelAlpha can write the Fastly DNS records
- PanelAlpha **v1.9.5** or later with the Fastly integration available

## Enable the Integration

Fastly starts as a hidden integration. Unhide it on the PanelAlpha server, then configure it in the Admin Area:

```bash
php artisan integrations:show fastly
```

1. Go to **Admin Area → Integrations**.
2. Find **Fastly**, enable it, and open **Configure**.
3. Paste the Fastly **API Token**.
4. Run **Test Connection**.

## Plan Configuration

After Fastly is enabled, open **Configuration → Plans → DNS** and expand **CDN Configuration**.

Select **Fastly** as the CDN provider and configure:

| Setting | Description |
|---------|-------------|
| **WAF** | Enables Fastly Next-Gen WAF for the service (administrator-managed) |
| **Force HTTPS** | Redirects HTTP to HTTPS at the edge |
| **Origin Shield** | Adds an Origin Shield location in front of the origin |
| **Shield Location** | Shield POP (for example Amsterdam, Frankfurt, London, Newark, San Jose, Singapore, Tokyo, Sydney) |
| **Under Attack Mode** | Default under-attack state for new instances |
| **Auto Purge on Update** | Purges Fastly cache after WordPress content updates via the MU-plugin |
| **Rate Limit (Normal / Attack)** | Request limits applied by edge configuration |
| **Cache TTL (HTML / Static)** | Cache lifetimes for HTML and static assets |

**Note:** Fastly is a CDN provider, not a DNS server. Keep the plan DNS provider set to your existing DNS integration. PanelAlpha writes Fastly edge records into that DNS provider.

## DNS instructions for customers

From **v1.9.8**, when Fastly is provisioned on the instance, **Set Your Domain** shows Fastly edge records, not the origin server IP.

- Apex `A` → Fastly anycast IPv4
- `www` `CNAME` → the Fastly hostname (for example `dualstack.*.fastly.net`)
- ACME CNAME when Fastly TLS validation needs it

DNS verification compares public DNS to those Fastly records. Pointing the domain at the origin IP bypasses the CDN.

This applies when the customer manages DNS at their registrar as well as when PanelAlpha writes the zone.

See [Changing Domain](../client-area-instances/changing-domain.md#set-dns-records).

## How Provisioning Works

When an instance is created (or Fastly is attached to an existing site) on a plan with CDN = Fastly, PanelAlpha:

1. Creates a Fastly service and configures the origin backend.
2. Applies edge settings (VCL / dictionary), including cache rules for WordPress admin, login, and WooCommerce paths that must bypass cache.
3. Provisions TLS and resolves the public DNS records Fastly expects.
4. Syncs those records to the plan DNS provider so the apex points to Fastly anycast addresses instead of the origin IP.
5. Attempts to enable WAF and install the CDN MU-plugin (best-effort steps that may log warnings if Fastly/WAF prerequisites are missing).

## Client Area Controls

Clients with the **Manage CDN** privilege can open **Instance → Advanced → CDN** when the plan CDN provider is Fastly and CDN is active for the instance.

Available actions:

- **Purge Cache** — purges the Fastly cache for the website
- **Under Attack Mode** — toggles the stronger edge protection profile
- **Always-On Protection** — read-only list of plan features visible to clients (for example Origin Shield, Auto purge on update)

## Related Pages

- [Plans — DNS](../system-configuration/plans.md#dns)
- [Advanced Settings](../client-area-instances/advanced-settings.md#cdn)
- [Integrations](index.md)
