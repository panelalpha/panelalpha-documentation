---
title: cPanel DNSOnly
description: Configure cPanel DNSOnly as a DNS server in PanelAlpha and connect it to your plans.
nav:
  group: DNS Servers
  order: 3
  title: cPanel DNS Only
---

# cPanel DNSOnly

PanelAlpha users can manage DNS zones and records using the **DNS Zone Editor** in the client area. To enable this, set up a DNS server in the plan.

If cPanel, DirectAdmin, or Plesk is set up as a DNS server, you do not need a separate DNS server, as these can function as the hosting, email, and DNS server simultaneously.

If **PanelAlpha Engine** is the hosting server, configuring cPanel DNSOnly (or another external DNS server) is mandatory.

## Configuration

To configure cPanel DNSOnly, go to **Admin Panel → Configuration → Servers → DNS Servers**.

Press **Add DNS Server**, and a short form will appear.

Enter the DNS server name and select **cPanel DNSOnly**. Then provide:

- cPanel server IP Address or Hostname
- cPanel server Username
- API Token — Learn how to create it [here](https://docs.cpanel.net/cpanel/security/manage-api-tokens-in-cpanel/)
- Nameservers — Provide if necessary

Test the connection and press **Add DNS Server**.

cPanel DNSOnly is now ready. Set it as the DNS server per plan under **PanelAlpha admin area → Configuration → Plans → Edit Plan → DNS**.
