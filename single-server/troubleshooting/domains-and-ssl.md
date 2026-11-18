---
title: "Domains and SSL"
description: "Problems when you add a domain, change the main domain or get an SSL certificate, with what each message means and what to do."
nav:
  group: Troubleshooting
  order: 4
---

# Domains and SSL

Problems when you add a domain, change the main domain or get an SSL certificate, with what each message means and what to do.

## Domains and SSL

**"Domain cannot start with 'www.'"** Enter `example.com`. The panel adds `www` for you.

**"Domain already exists."** (in **Change Domain** the message names the domain: `example.com is already on this engine.`) Another project on this server already has that domain. Remove it there first, or use another name.

**"Domain Name must be valid"** (in **Add Domain**: `Domain must be valid`; when the server refuses the name: `Invalid domain name.`) The text is not a domain name. Enter it as `example.com`, with no path and no `www.`.

**"Cannot delete main domain"** The main domain has no delete icon. Make another domain the main one first, then remove this one.

**"Cannot delete domain with existing subdomains"** Remove the subdomains first, then remove the domain.

**The SSL Status column says `Wrong DNS`.** The domain's A record (the DNS entry that points a name at a server) points at another server. Point it at this server. A domain behind a service such as Cloudflare can show this too: [Domains and SSL](../looking-after-a-project/domains-and-ssl.md#ssl-certificates).

**"Invalid certificate/key"** The certificate and the private key do not belong together. Paste the key that was made for this certificate.

**"Could not obtain a certificate for <name>: ... is not a public hostname a certificate authority will issue for"** The name is not a public domain name. Use a domain you own.

More about this: [Domains and SSL](../looking-after-a-project/domains-and-ssl.md).
