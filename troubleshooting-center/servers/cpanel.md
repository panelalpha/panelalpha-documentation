---
title: cPanel
description: Fix PanelAlpha cPanel issues including subdomain errors, domain IP mismatches, out of IP addresses, and raw log errors.
nav:
  group: Server Issues
  order: 5
---


# cPanel

This page covers common cPanel-related issues in PanelAlpha.

### Error During Reinstallation of an Instance

**Problem:**
```
Error: The domain subdomain.example.com may not be created by Y because example.com is already owned by another user.
```

**Solution:** Enable **Allow cPanel users to create subdomains across accounts** in [cPanel Tweak Settings](https://support.cpanel.net/hc/en-us/articles/360063525253-How-to-allow-users-to-create-subdomains-for-a-domain-owned-by-another-cPanel-account).

### Domain Points to an IP Address Without DNS Servers

**Problem:** The domain points to an IP address that does not use the associated DNS servers.

**Solution:**
1. If you are using the quick onboarding method, ensure the onboarding domain has a wildcard DNS record pointing to your hosting server's IP address.

   **Or:**

2. Enable **Allow remote domains** in cPanel's Tweak Settings. For details, see the [cPanel Support page](https://support.cpanel.net/hc/en-us/articles/360050075034-This-domain-points-to-an-IP-address-that-does-not-use-the-DNS-servers-associated-with-this-server).

### Out of Available IP Addresses

**Problem:** No more available IP addresses on the system.

**Solution:** Refer to the [cPanel documentation](https://support.cpanel.net/hc/en-us/articles/360060530234-Cannot-create-a-new-account-with-package-that-uses-a-dedicated-IP-address) for guidance on resolving this issue when assigning packages with dedicated IP addresses.

### "Refresh Report Data" Error

**Problem:** The **Refresh Report Data** action fails with the error: `You do not have the features 'rawlog'`.

**Solution:** Ensure **Raw Access Log** is enabled in cPanel/WHM Feature Manager.

### DNS Entry Already Exists During Migration

**Problem:** A migration fails with the error:
```
A DNS Entry For 'example.com' Already Exists. You Must Remove This DNS Entry From This Server Or All Servers In The DNS Cluster To Proceed.
```

**Cause:** A DNS zone for the domain already exists in cPanel/WHM on the destination server (or across the DNS cluster), which prevents the migration from creating a new account for that domain.

**Solution:**
1. Log in to **WHM** on the destination server.
2. Navigate to **DNS Functions → Delete a DNS Zone**.
3. Find and remove the DNS zone for the conflicting domain.
4. If you are using a DNS cluster, ensure the zone is removed from **all servers** in the cluster.
5. Once the DNS entry is deleted, restart the migration process in PanelAlpha.

## See Also

- [cPanel hosting server setup](../../multi-server-edition/hosting-servers/cpanel.md)
- [Common Error Solutions](../common/common-error-solutions.md)
