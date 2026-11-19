---
title: Client Area Functionality
description: Resolve PanelAlpha client area issues including WP-Admin login errors, unauthorized access, outdated instance details, and missing previews.
nav:
  group: Client Area Issues
  order: 1
  title: Client Functionality
---


# Client Area Functionality

### Issues Logging into WP-Admin

**Problem:** The following error appears when trying to log into WP-Admin:

```
Warning: The package could not be installed. 'No valid plugins were found.' Plugin update failed.
Warning: The '/home//.panelalpha/.../controlpanel-sso.zip' plugin could not be found.
```

**Solution:** This error occurs when the Control Panel Connection must-use plugin is missing or corrupted. Run the following command on the PanelAlpha server:

```bash
docker compose -f /opt/panelalpha/app/docker-compose.yml exec api php artisan connectors:build
```

The **SyncWordPressData** job will automatically reinstall the must-use plugin during its next daily run. To force immediate installation, manually trigger the synchronization or wait for the scheduled job.

### Unauthorized or Not Authorized Message in Client Area

**Problem:** Users see an `Unauthorized` or `Not authorized` message in the Client Area.

**Solution:** Check the plan configuration under **Plan → Other → Disable privileges** for the affected plan. If this option is enabled, disable it and test access again.

### Visitors and Bandwidth Not Calculated

**Problem:** The number of website visitors and bandwidth is not calculated or displayed in the instance details.

**Solution:** When using PanelAlpha Engine, visitor and bandwidth data will not display unless you configure the [Matomo](../../multi-server-edition/integrations/matomo.md) or [Google Analytics](../../multi-server-edition/integrations/google-analytics.md) integrations.

### Instance Details Are Outdated or Out of Sync

**Problem:** Instance details do not reflect the actual state of the WordPress installation.

**Solution:** Run the synchronization command on your PanelAlpha server to refresh instance details:

```bash
docker compose -f /opt/panelalpha/app/docker-compose.yml exec -T api php artisan applications:sync-installation-details {id?}
```

**Parameters:**
- `{id?}` (optional): Specific instance ID to sync. If omitted, all instances will be synchronized.

### Overwrite WordPress Connector Settings on All Instances

**Problem:** The Control Panel connector settings are out of sync or need to be re-applied across instances (for example, after changing connector configuration or troubleshooting connector issues).

> [!WARNING]
> **Warning:** Running `applications:sync-wordpress-data --apply-default-settings` without an instance ID affects ALL instances. Specify `<id>` to test on a single instance first.

**Solution:** Run the following command on the PanelAlpha server to perform WordPress synchronization and overwrite connector settings:

```bash
docker compose -f /opt/panelalpha/app/docker-compose.yml exec api php artisan applications:sync-wordpress-data [--install] [--apply-default-settings] [<id>]
```

**Parameters:**
- `<id>` (optional): Specific application ID to sync. If omitted, the command runs for all applications.
- `--install`: Force install (reinstall) of the Control Panel connector.
- `--apply-default-settings`: Push feed with default settings.

### Instance Preview Not Available

**Problem:** The website preview in the "My Instances" section shows "Preview not available."

**Solution:** Enable and configure the **Google PageSpeed Insights** integration. For more information, see the [Google PageSpeed Insights](../../multi-server-edition/integrations/google-pagespeed-insights.md) documentation.

### Domain Does Not Point to the Instance

**Problem:** The domain does not point to the server after instance creation.

**Solution:**
1. Check if a "DNS propagation is in progress" message appears in the Client Area.
2. Click the message for instructions on properly mapping your domain to the server.

### Custom Logo Image

**Question:** The logo appears pixelated. What are the standard dimensions for the logo?

**Answer:** The recommended dimensions for the PanelAlpha logo are 271x60 pixels. To prevent pixelation, use an `.svg` file, which scales without losing quality.
