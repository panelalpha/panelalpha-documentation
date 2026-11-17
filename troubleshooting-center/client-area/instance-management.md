---
title: Instance Management Issues
description: Fix PanelAlpha instance management issues including creation errors, domain problems, staging failures, and push-to-live errors.
nav:
  group: Client Area Issues
  order: 2
  title: Instance Management
---


# Instance Management Issues

## Instance Creation Problems

Common issues related to creating new instances (instance creation failing, domain validation errors, and hosting account creation issues) are covered in [Client Functionality](client-functionality.md).

### "The Domain Name Is Not Available" Error

**Problem:** The error message **"The domain name is not available"** appears when trying to create a domain during instance creation.

**Solution:** This error is typically caused by the **Block Profanity** setting in your plan configuration.

1. Navigate to **Configuration → Plans** and select the plan you are using.
2. Under the **Domains** tab, check the **Subdomain Blacklist** and **Domains Blacklist** sections.
3. If **Block Profanity** is enabled, the domain name may contain words flagged as profanity and therefore blocked.

You have two options:
- Choose a different domain name that does not contain blocked profanity terms.
- Contact your administrator to review and adjust the profanity blacklist settings.

For more details, see the [Plans documentation](../../multi-server/system-configuration/plans.md#domains).

## Managing Existing Instances

Problems with managing existing instances (WP-Admin access, updates not applying, and plugin/theme installation issues) are covered in [Client Functionality](client-functionality.md).

## Backup and Restore Problems

Issues with backup and restore functionality (backups failing to create, restore failures, and scheduled backup issues) are covered in [Backup Issues](../backups/backup-issues.md) and [Snapshot Tool Issues](../backups/snapshot-tool-issues.md).

## Staging and Push-to-Live Issues

### Database Privilege Errors (SUPER/SET_USER_ID)

**Problem:** Pushing staging to live fails with MySQL errors such as `ERROR 1227 (42000)` requiring `SUPER` or `SET_USER_ID`.

**Solution:** Run the push from an account with the required MySQL privileges, or ask the hosting provider to grant the privileges temporarily.

### Missing Media or Content After Push

**Problem:** After the push, files or page content are missing.

**Solution:** Re-run the push with both **Overwrite files** and **Push full database** enabled to sync files and data together.

### Proxy Interference (Cloudflare)

**Problem:** Push or staging tasks time out when DNS is proxied.

**Solution:** Temporarily disable the Cloudflare proxy (gray cloud) for the involved hostnames and retry the task. For webhook/proxy timeout background, see [Cloudflare](../servers/cloudflare.md#webhook-requests-timing-out-when-cloudflare-proxy-is-enabled).

## See Also

- [Client Functionality](client-functionality.md)
- [Backup Issues](../backups/backup-issues.md)
- [Snapshot Tool Issues](../backups/snapshot-tool-issues.md)
- [Cloudflare](../servers/cloudflare.md)
