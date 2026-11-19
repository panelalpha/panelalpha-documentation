---
title: Plans Configuration
description: Fix PanelAlpha plan configuration issues including DNS server limits, IP assignment, subdomain creation, and memory limits.
nav:
  group: Configuration Issues
  order: 1
---


# Plans Configuration

## FAQ

### Using More Than One DNS Server

**Question:** Can I assign multiple DNS servers when setting up a plan?

**Answer:** PanelAlpha allows only one DNS server per plan. To have backup DNS servers, you can use cPanel's DNS synchronization. For more details, see the [cPanel documentation](https://support.cpanel.net/hc/en-us/articles/4419555120407-How-to-Syncronize-DNS-records-to-a-cPanel-DNS-Cluster).

### Error When Installing an Instance

**Question:** Every time I create an instance, Plesk uses the dedicated IP. How can I set it to use the shared IP by default?

**Answer:** Select the IP address in the **Hosting** tab of your plan configuration. Selecting the shared IP type should resolve this issue.

### Automatically Creating Subdomains

**Question:** Is it possible for a client to automatically create a subdomain during the order process?

**Answer:** Yes. Use the Quick Onboarding or Super Quick Onboarding methods and:
- Ensure the **Ask for domain** option is disabled.
- Set the main domain in the **Domain used for subdomain** field.
- Select the desired configuration in the **Automatic subdomain format** options.

## Troubleshooting

### Memory Limit in PanelAlpha Engine

**Problem:** Error: `Error 137: Out of Memory`

**Solution:** Increase the memory limit in the plan configuration and upgrade the plan. In the admin area, select **Upgrade** and reapply the same plan.

### Changing Server IP Address vs. Changing IP on Services

**Problem:** After changing the server IP (PanelAlpha Engine), the associated services do not reflect the new IP address.

> [!WARNING]
> **Warning:** Removing and recreating services deletes associated client data. Back up before proceeding.

**Solution:** IP address changes apply only to newly created services. Existing services will not be updated unless they are removed and recreated.

### Changing DNS Server in Plan Configuration

**Problem:** After changing the DNS server and selecting **Create A record on parent zone** under **Subdomain DNS Action**, issues with creating instances on subdomains persist.

> [!WARNING]
> **Warning:** Removing and recreating services deletes associated client data. Back up before proceeding.

**Solution:** DNS server changes in the plan configuration apply only to newly created services. To apply these changes to existing services, they must be removed and recreated.

### CPU Limit Exceeds Available Resources

**Problem:** Error response from daemon: `Range of CPUs is from 0.01 to 2.00, as there are only 2 CPUs available`

**Solution:** Ensure the CPU limit in the **Hosting** tab of the plan is lower than the number of CPUs on your PanelAlpha Engine server.

### Tuning PHP-FPM Worker Count per PanelAlpha Engine Plan

**Problem:** You need to adjust the number of PHP workers for a specific plan without editing the shared PHP-FPM node configuration.

**Solution:** Use the **PHP-FPM Pool Settings** field inside the plan's hosting configuration and provide a JSON-style override such as:

```json
{"pm.max_children": 30}
```

PanelAlpha Engine injects that override directly into the PHP-FPM pool for every instance created from the plan, which controls `pm.max_children` (the number of PHP worker processes) without touching the global `/etc/php-fpm.d` templates.
