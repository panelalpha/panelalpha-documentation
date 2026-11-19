---
title: Nameserver Changes and DNS Migration
description: Understand what happens to your DNS records when you change nameservers in PanelAlpha and how to recreate them manually.
nav:
  group: Client Area - Instances
  order: 6
---

# Nameserver Changes and DNS Migration

Switching nameservers moves DNS authority to DNS server, but it **does not transfer your existing DNS records**. Records from your previous provider stop working the moment the change propagates. You need to recreate them manually.

## Before You Change Nameservers

Log in to your current DNS provider and write down (or export) every record in the zone. Pay close attention to:

- **MX records** — point to your mail server. Losing these means incoming email stops working immediately after the switch.
- **TXT records** — include SPF, DKIM, and DMARC entries for email authentication, as well as any domain verification records (Google Search Console, Microsoft 365, etc.).
- **A / CNAME records for subdomains** — for example `mail.yourdomain.com`, `ftp.yourdomain.com`, or any subdomain pointing to a third-party service.
- **SRV / CAA records** — less common but used by some services (VoIP, certificate authorities).

Most DNS providers let you view all records in a table or export the zone as a text file. Do this before making any changes.

## Changing Nameservers

When you switch nameservers at your registrar, the change propagates gradually — it can take anywhere from a few minutes to 48 hours depending on the registrar and the TTL of the existing NS records. During propagation, some visitors will still resolve through the old provider while others already use the new one.

**Note:** Minimize downtime
Before switching, lower the TTL on your existing DNS records to 300 seconds (5 minutes) and wait for that TTL to expire. That way, once you switch, old cached responses expire quickly.

## After You Change Nameservers

Once propagation is complete, open the [DNS Zone Editor](../client-area-hosting/dns-zone-editor.md) in PanelAlpha and recreate the records you noted down earlier:

1. Navigate to **Client Area → Hosting → DNS Zone Editor**.
2. Add MX records first — email is the most time-sensitive service.
3. Add TXT records (SPF, DKIM, DMARC, and any verification entries).
4. Add any A or CNAME records for subdomains.

PanelAlpha automatically creates the A record for your WordPress instance and the `www` CNAME. You do not need to add those.

After adding the records, verify that email is working and that any third-party services depending on DNS entries are responding correctly.

## Useful Resources

- [Bunny DNS — Update nameservers at your registrar](https://docs.bunny.net/dns/nameservers#update-nameservers-at-your-registrar)
- [Cloudflare — Update nameservers when your domain uses a different registrar](https://developers.cloudflare.com/dns/nameservers/update-nameservers/#your-domain-uses-a-different-registrar)
