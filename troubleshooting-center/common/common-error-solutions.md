---
title: Common Error Solutions
description: Fix common PanelAlpha errors including DNS zone mismatches, missing SSL orders, and certificate issues.
nav:
  group: Common Issues
  order: 1
---


# Common Error Solutions

## DNS Zone Issues

### DNS Zone Manually Deleted on DNS Server

**Problem:** A DNS zone was manually deleted directly on the DNS server (Cloudflare, cPanel, PowerDNS, etc.) outside of PanelAlpha, causing a mismatch between PanelAlpha's database and the actual DNS server state.

**Symptoms:**
- PanelAlpha shows a zone exists but the DNS server reports it as missing
- Unable to create a new zone for the same domain
- Zone synchronization errors in logs

**Solution:** Delete the orphaned zone record from PanelAlpha's Admin Area:

1. Navigate to **Admin Area → Services** and find the affected service.
2. Click the service to open its details page.
3. Go to the **DNS Zones** tab.
4. Locate the orphaned zone.
5. Click the **delete icon** (trash) next to the zone.
6. Check the confirmation checkbox and click **Delete**.

**What happens when you delete:**
- The zone record is removed from PanelAlpha's database.
- PanelAlpha attempts to delete the zone from the DNS server (gracefully handles if already deleted).
- You can then manually create a new zone on the DNS server and import it into PanelAlpha via **Admin Area → Configuration → Servers → DNS Servers → select server → Synchronize → Import Zone**.

**Tip:** This feature is useful for:
- Cleaning up after manual DNS server maintenance
- Resolving zone synchronization issues
- Preparing to reassign zones with different configurations

## SSL Certificate Issues

### Issue SSL Orders for Domains Without Existing Orders

**Problem:** You have domains that require SSL certificates, but no SSL orders have been created. This can happen when domains are added before automatic SSL is configured.

**Solution:** Run the following command to create SSL orders for all eligible domains without an existing order:

```bash
docker compose -f /opt/panelalpha/app/docker-compose.yml exec -T api php artisan automatic-ssl:create-order
```

This command will:
- Scan all eligible domains in the system
- Check which domains do not have an existing SSL order
- Create new SSL orders for those domains
- Queue the orders for processing by the SSL automation system

**Note:** Make sure your automatic SSL configuration is properly set up before running this command. The orders will be processed according to your SSL provider settings.

## Need More Help?

If the solutions above do not resolve your issue, refer to the [Troubleshooting Center](../index.md) for additional guidance or contact [PanelAlpha Support](https://manage.panelalpha.com/contact).
