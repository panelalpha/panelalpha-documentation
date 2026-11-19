---
title: "Domains and SSL"
description: "A project answers on one main domain. This page shows how to use your own domain, add more domains and give each one an SSL certificate."
nav:
  group: Looking After a Project
  order: 1
---

# Domains and SSL

A project answers on one main domain. You can use your own domain, add more domains, redirect a domain and give each domain an SSL certificate, which makes visitors see the padlock. You do this in the project under **Advanced > Domains** and **Advanced > Security**.

## Pointing your own domain at a project

New projects get an address from the default domain set in the panel: [Settings](../the-panel/settings.md). To use your own domain as the project's main domain:

1. Open the project and select **Change Domain** next to the project address at the top of the page. While the domain does not point at this server, the button reads **DNS propagation is in progress** instead and opens the same dialog.
2. **Set Your Domain** shows the current address as **Your domain**. Select **Change**. On a `panelalpha.online` address the dialog warns that this address will be gone for good.
3. Open **Map your own domain** and type the domain in **Domain name**, for example `shop.example.com` for Bagisto. The name `example.com` is only a stand-in in this guide. Use your own domain.
4. Select **Use This Domain**. The panel changes the project's address at once. The dialog then shows **Set DNS Records**.

   **DNS** is the system that tells the internet which server a domain name lives on. A **DNS record** is one line of that information. You set the records where you bought the domain (your domain provider). The dialog lists two:

   - Name `shop.example.com` (or `@`), type `A`, address the IPv4 address of your server. An **A record** points a name at a server address. The dialog shows the address.
   - Name `www.shop.example.com`, type `CNAME`, address `shop.example.com`. A **CNAME record** makes one name follow another.

   The dialog also holds **Change**, to pick another domain, **Generate URL**, a temporary address to preview the site, and **Close**.

   :docs-video{src="../../assets/single-server-edition/domains-set-your-domain-dns.webm" title="Bagisto on its free panelalpha.online address: Change Domain opens Set Your Domain, Change, the domain shop.example.com typed into Map your own domain, Use This Domain, and Set DNS Records with the A record and the CNAME record for www"}

5. Set these records at your domain provider and wait for them to spread. This takes from 2 minutes to a few hours.

To see the records again, open **Change Domain** on the project.

**Changing the main domain changes the project's address, not the project.** The address changes. The files stay. A WordPress site keeps working, including **WP Admin**. The visitor statistics start again from zero.

**Changing the main domain away from a `panelalpha.online` address removes that address for good.** The dialog warns you first.

The other option in **Set Your Domain** is **Use a free subdomain**, which gives the project an address under a base domain your engine offers. If the address is refused, you see `Domain name is not available.` Choose another name.

The panel refuses a name that is not a domain (`Domain Name must be valid`) and a name that starts with `www.` (`Domain cannot start with 'www.'`). Enter `example.com`. The panel adds `www` itself.

### When the domain does not point at the server

The panel checks from your browser where the domain points. If the domain does not point at this server, the top bar of the project shows **DNS propagation is in progress** and **My Projects** shows a warning icon next to the project's address. Point at the icon to read the reason: `The A record may be missing, pointing elsewhere, or routed through a proxy such as Cloudflare.` It offers two choices:

- **Fix it now** opens **Set Your Domain** with the records to set.
- **Ignore** turns the warning off for this project. **Restore Alerts** turns it back on.

If DNS cannot point at the server yet, **Set Your Domain** also offers **Generate URL** under `Not live yet? Preview the site on the server`. It gives a temporary address so you can look at the site first: [Staging and preview](../projects/staging-and-preview.md).

::docs-video{src="../../assets/single-server-edition/domains-dns-warning.webm" title="My Projects: pointing at the warning icon next to shop.example.com on the Bagisto card shows The A record may be missing, pointing elsewhere, or routed through a proxy such as Cloudflare, with the buttons Fix it now and Ignore"}
::

## Adding a domain

An **addon domain** is a second, separate domain on the same project, for example `example.org` beside `example.com`. A **subdomain** is a name in front of a domain you already have, for example `shop.example.com`.

**Advanced > Domains** lists every domain of the project. The columns are:

- **Domain:** the name.
- **SSL Status:** who issued its certificate, or `Self Signed`, `No SSL` or `Wrong DNS`: [SSL certificates](#ssl-certificates).
- **Type:** **Main Domain** is the address of the project. **Addon Domain** is a second domain. **Subdomain** is a name in front of a domain.
- **Points to:** the folder the domain serves, also called the document root.
- **Actions:** the star makes an addon domain the main one, the padlock opens the certificate window (**Edit SSL**), the pencil edits the row and the bin removes it. The main domain has no pencil and no bin.

To add a domain:

1. Open the project, go to **Advanced > Domains** and select **Add Domain**.
2. In **Add New Domain**, enter the **Domain**. The field starts with `www.`, which the panel adds itself, so do not type it.
3. In **Type**, choose **Addon Domain** or **Subdomain**. For a subdomain, enter the name and choose the parent domain, for example `shop` under `example.com`.
4. Select **Add Domain**. Select **Cancel** to close the window without a new domain.

::docs-video{src="../../assets/single-server-edition/domains-list.webm" title="Advanced > Domains of Immich with the columns Domain, SSL Status, Type and Points to: Add Domain opens Add New Domain with the type Addon Domain, example.org is added, and the pointer stops on Edit SSL in its row"}
::

Set the DNS records for the new domain at your domain provider, as above.

## Changing a domain, redirecting it or removing it

1. Go to **Advanced > Domains** and open the edit action of the row.
2. Change the **Document Root**, which is the folder the domain serves. It starts with `/`.
3. To send visitors elsewhere, tick **Redirect Domain** and enter the **Redirect URL**.
4. Select **Update**.

To remove a domain, use the delete action of the row and tick the confirmation. **The main domain cannot be removed.**

## Making an addon domain the primary one

The primary domain is the project's main domain.

1. Go to **Advanced > Domains** and select the star on the row of an addon domain.
2. Confirm **Set As Primary Domain**.

The old main domain stays in the project as an addon domain and keeps serving the site. A WordPress site keeps working on the new main domain. The new main domain starts again without the visitor statistics it had.

**The panel does not add a redirect to the new primary domain.** To send visitors of an old domain to the new one, tick **Redirect Domain** on its row.

## SSL certificates

Each domain has a certificate. It comes from Let's Encrypt or from you, or it is self-signed, which browsers do not trust. The **SSL Status** column in **Advanced > Domains** shows the issuer, `Self Signed`, `No SSL` or `Wrong DNS`.

**`Wrong DNS` means the domain points at another server, or sits behind the Cloudflare proxy without a tunnel.** A `panelalpha.online` address or a domain served through its own tunnel, such as a Cloudflare tunnel, does not show it. Fix the domain's A record: [When the domain does not point at the server](#when-the-domain-does-not-point-at-the-server).

1. Go to **Advanced > Domains** and select the padlock icon on the row. You can also use **Advanced > Security**, card **SSL Certificate Applied**, button **Manage**.
2. **Set SSL Certificate** names the **Domain**. Under it, **SSL Certificate installed on server** shows the certificate the panel put on the server. **Detected SSL Certificate** shows the one a browser finds on the domain. They can differ, for example while a new certificate is not live yet. Then choose the **SSL provider**: **Let's Encrypt** or **Custom Certificate**.
3. For **Let's Encrypt**, select **Generate**. The domain must already point at the server, or the certificate waits for DNS.
4. For a certificate of your own, fill in **Certificate (CRT)** and **Private Key (KEY)**. **Certificate Authority Bundle (CABUNDLE)** is optional. Select **Install**.

::docs-video{src="../../assets/single-server-edition/domains-ssl-dialog.webm" title="The padlock of photos.example.com in Advanced > Domains of Immich opens Set SSL Certificate: the installed and detected certificates, the SSL provider list switched from Let's Encrypt to Custom Certificate, which shows Certificate (CRT) and Private Key (KEY)"}
::

If installation fails, check that the DNS records are right and try again.

Names under `panelalpha.direct`, `panelalpha.online`, `sslip.io` and `nip.io` get no Let's Encrypt certificate from the panel unless you allow it on the server: [On the server](../getting-started/on-the-server.md#certificates-for-shared-names).

### Removing an installed certificate

**The certificate you replace is not kept, and neither is its private key.** Save the CRT and the KEY first if you want them again.

1. Open the padlock of the domain as above.
2. Select **Remove Certificate**, then confirm with **Remove**.

The domain goes back to the default certificate the server uses for new domains. That certificate is usually self-signed, so the browser warning comes back and the **SSL Status** column reads `Self Signed`.

### Forcing HTTPS

1. Go to **Advanced > Security**.
2. Turn on **Force HTTPS Redirect**.

This works only when the domain has a certificate. If other projects share the same domain, the panel lists them first, because the change affects them too.

Something not working? Messages and fixes: [Troubleshooting: Domains and SSL](../troubleshooting/domains-and-ssl.md).
