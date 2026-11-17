---
title: "Domains and SSL"
description: "A project answers on one main domain. You can point your own domain at it, add more domains and subdomains, redirect a domain, and give each domain an SSL..."
nav:
  group: Looking After a Project
  order: 1
---

# Domains and SSL

A project answers on one main domain. You can point your own domain at it, add more domains and subdomains, redirect a domain, and give each domain an SSL certificate so visitors see the padlock. These pages are in the project under **Advanced > Domains** and **Advanced > Security**.

## Pointing your own domain at a project

New projects get an address from the default domain set in the panel: [Settings](../the-panel/settings.md). To use your own domain as the project's main domain:

1. Open the project and select **Change Domain** next to the project address at the top of the page.
2. In **Set Your Domain**, choose **Map your own domain** and enter the domain, for example `example.com`.
3. Select **Use This Domain**. The panel changes the project's address at once, and the dialog then shows **Set DNS Records** with the records to set at your domain provider:

   - Name `example.com` (or `@`), type `A`, address the IPv4 address of your server. The dialog shows the address.
   - Name `www.example.com`, type `CNAME`, address `example.com`.

   <!-- TODO screenshot: assets/domains-set-your-domain-dns.png. Show the Set Your Domain dialog after Use This Domain: Your domain example.com on the config step, and the open Set DNS Records section with the A record (203.0.113.10) and the CNAME record for www.example.com. Crop to the dialog. -->

4. Set these records. Wait for them to spread. This takes from 2 minutes to a few hours.

The same records show when you open **Change Domain** again on a project that already has a domain.

**Making another domain the main one changes the project's address, not the project.** The project keeps its name in My Projects and on the server: its account, its home directory and its files stay as they are, and only its main domain and the address it answers on change. The new main domain serves the same folder the old one served, and a WordPress site's addresses are updated, so its pages and **WP Admin** go on working. The project's custom proxy rules move to the new main domain, but its tunnel and its visitor statistics do not: a tunnel serves one domain name, and the statistics the panel shows are the main domain's, so they start again from zero.

A `panelalpha.online` address is served through such a tunnel, so **changing the main domain away from it removes that address for good**: the Engine does not make a `panelalpha.online` name a project's main domain again. The dialog warns you before you change it.

The other option in **Set Your Domain** is **Use a free subdomain**, which gives the project an address under a base domain your Engine offers. If the address is refused, you see `Domain name is not available.` Choose another name.

The panel checks the name before it changes anything: `Domain Name must be valid` under the field means the text is not a domain, and a name that starts with `www.` is refused with `Domain cannot start with 'www.'`: enter `example.com`, the panel adds `www` itself. A project that the server owner has protected cannot be changed: `The server operator has protected this project, so it cannot be deleted, stopped or redeployed from the panel.`

### When the domain does not point at the server

The panel warns: `The A record may be missing, pointing elsewhere, or routed through a proxy such as Cloudflare.` Select **Fix it now** and set the records above. If DNS cannot point at the server yet, the dialog can make a temporary address so you can look at the site first: [Staging and preview](../projects/staging-and-preview.md).

<!-- TODO screenshot: assets/domains-dns-warning.png. Show the warning about the A record with the Fix it now button. -->

## Adding a domain

1. Open the project, go to **Advanced > Domains** and select **Add Domain**.
2. In **Type**, choose **Addon Domain** or **Subdomain**.
3. For an addon domain, enter the domain without `www.`. The panel adds `www` itself. For a subdomain, enter the name and choose the parent domain, for example `shop` under `example.com`.
4. Select **Add Domain**.

<!-- TODO screenshot: assets/domains-list.png. Show Advanced, Domains with the main domain example.com, an addon domain and shop.example.com. Make visible the SSL column, the star, the edit action and the Add Domain button. -->

Set the DNS records for the new domain at your domain provider, as in the section above.

## Changing a domain, redirecting it or removing it

1. Go to **Advanced > Domains** and open the edit action of the row.
2. Change the **Document Root**, which is the folder the domain serves. It starts with `/`.
3. To send visitors elsewhere, tick **Redirect Domain** and enter the **Redirect URL**. The list then shows `redirected to <address>`.
4. Select **Update**.

To remove a domain, use the delete action of the row and tick the confirmation. **The main domain cannot be removed.**

## Making an addon domain the primary one

1. Go to **Advanced > Domains** and select the star on the row.
2. Confirm **Set As Primary Domain**.

**All other domains of the project keep the redirect settings they have; the panel does not add a redirect to the new primary domain.** To send the visitors of an old domain to the new one, tick **Redirect Domain** on its row, as in the section above. The domain that becomes the main one is removed from the project and added again, so it comes back without the tunnel, the custom proxy rules and the visitor statistics it had on its own. The old main domain stays in the project as an addon domain, with its files, so it goes on serving the site: the new main domain serves the same folder. A WordPress site's addresses are updated as on a domain change, so its pages and **WP Admin** go on working on the new main domain. The star is offered on the main domain and on addon domains only.

When the main domain is a `panelalpha.online` address, the panel keeps it working: it re-adds it as an addon domain together with its tunnel, so the address serves the site as before, and the dialog tells you so. That address cannot be the main domain again: the Engine refuses a `panelalpha.online` name as a main domain with `A PanelAlpha Online address is served through the proxy as a tunnel, so it cannot be the project's main domain.`

## SSL certificates

Each domain has a certificate: from Let's Encrypt, one you provide or a self-signed one that browsers do not trust. The **SSL Status** column in **Advanced > Domains** shows the issuer, `Self Signed` or `No SSL`.

1. Go to **Advanced > Domains** and select the padlock icon on the row (its tip reads **Edit SSL**). You can also use **Advanced > Security**, card **SSL Certificate Applied**, button **Manage**.
2. In **Set SSL Certificate**, choose the **SSL provider**: **Let's Encrypt** or **Custom Certificate**.
3. For **Let's Encrypt**, select **Generate**. The panel shows `The SSL certificate is being generated.` The domain must already point at the server, or the certificate waits for DNS.
4. For a certificate of your own, fill in **Certificate (CRT)** and **Private Key (KEY)**. **Certificate Authority Bundle (CABUNDLE)** is optional. Select **Install**. The panel shows `The SSL certificate has been installed.`

<!-- TODO screenshot: assets/domains-ssl-dialog.png. Show the Set SSL Certificate dialog with the SSL provider list, Let's Encrypt chosen and the Generate button. Include the Certificate (CRT) and Private Key (KEY) fields if they show for a custom certificate. -->

If installation fails the status reads `The SSL certificate installation for example.com has failed. Please contact support.` Check that the DNS records are right and try again.

A name under `panelalpha.direct`, `panelalpha.online`, `sslip.io` or `nip.io` cannot get a Let's Encrypt certificate from the panel, because many servers share these names and Let's Encrypt allows only 50 new certificates a week for each of them. The panel then shows an error that mentions `shared_zone_issuance`. Use a domain of your own, or ask the server owner to allow it with `pae settings:set ssl_shared_zone_issuance 1`.

### Removing an installed certificate

1. Go to **Advanced > Domains** and select the padlock icon on the row (its tip reads **Edit SSL**), or use **Advanced > Security**, card **SSL Certificate Applied**, button **Manage**.
2. Select **Remove Certificate**, then confirm with **Remove**.

The certificate you replace is **not kept, and neither is its private key**: save the CRT and the KEY first if you want them again. The domain goes back to the certificate the server gives a new domain: its own one where that covers the name, otherwise the one its operator configured, which is self-signed by default. A self-signed certificate brings the browser warning back, and the **SSL Status** column reads `Self Signed`.

The dialog shows the certificate the domain carries now as soon as the server confirms the removal, and every page that is open for the project updates. On a server whose Engine cannot do this yet, the dialog answers `This server's Engine cannot remove a certificate yet.`

### Forcing HTTPS

1. Go to **Advanced > Security**.
2. Turn on **Force HTTPS Redirect**.

This works only when the domain has a certificate. Other projects can share the same domain. If so, the panel lists them first: `This project is installed on example.com along with other projects. Changing this option, it will also affect the following projects:`.

## Troubleshooting

**"Domain cannot start with 'www.'"** You entered the address with `www.`. Enter `example.com`. The panel adds `www` for you.

**"Domain already exists."** (in **Change Domain** the answer names the domain: `example.com is already on this engine.`) Another project on this server already has that domain. Remove it there first or use another name.

**"Domain Name must be valid"** (in **Add Domain**: `Domain must be valid`; a name the server refuses: `Invalid domain name.`) The text is not a domain name. Enter it as `example.com`, without a path and without `www.`.

**"Cannot delete main domain"** The main domain has no delete icon. To remove it, make another domain the main one first, then remove this one.

**"Cannot delete domain with existing subdomains"** Remove the subdomains first, then remove the domain.

**"Invalid certificate/key"** The certificate and the private key do not belong together. Check that you pasted the key of this certificate.

**"Could not obtain a certificate for <name>: ... is not a public hostname a certificate authority will issue for"** The name is not a public domain name. Use a domain you own.
