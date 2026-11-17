---
title: Diagnostic Mode
description: Use PanelAlpha Diagnostic Mode to resolve hosting account not found, missing instance files, and manually disable diagnostic mode.
nav:
  group: Diagnostic Issues
  order: 1
---


# Diagnostic Mode

## Hosting Account Not Found

**Problem:** The hosting account has not been detected. It may have been removed.

**Solutions for the administrator:**

> [!WARNING]
> **Warning:** Deleting a service removes associated client data. Back up the service before deleting and recreate it carefully.

1. Delete the service manually from PanelAlpha.
2. Recreate the service manually. You will need the username associated with the missing hosting account. If you have a backup of the WordPress installation from that service, you may also restore it.

### How to Restore a Hosting Service

1. Find the username associated with the missing hosting account:

   ![PanelAlpha Troubleshooting — Diagnostics — Locating the username of a missing hosting account](../../assets/img/Troubleshooting-Center/Diagnostic_Mode_1.png)

2. Manually create a service with the previously used username on the **hosting server**.

3. If WordPress instances were previously installed on the corrupted hosting service, restore them using backups:

   ![PanelAlpha Troubleshooting — Diagnostics — Restoring WordPress instances from backup](../../assets/img/Troubleshooting-Center/Diagnostic_Mode_2.png)

   ![PanelAlpha Troubleshooting — Diagnostics — Selecting a backup to restore the hosting service](../../assets/img/Troubleshooting-Center/Diagnostic_Mode_3.png)

## Instance Files Not Found

**Problem:** The WordPress installation files could not be detected.

**Solution:** Restore the WordPress installation from a backup.

See [Snapshot Tool Issues](../backups/snapshot-tool-issues.md) for restore procedures.

   ![PanelAlpha Troubleshooting — Diagnostics — Restoring instance files from backup](../../assets/img/Troubleshooting-Center/Diagnostic_Mode_2.png)

## Manually Disabling Diagnostic Mode

**Problem:** You need to manually disable diagnostic mode for a specific instance. This is only necessary when Diagnostic Mode displays the message: "Diagnostic Mode has been enabled by admin."

You can also review active incidents and historical events in **Admin Area → Logs → Diagnostic Mode** (`/logs/diagnostic-mode`) using the **Current Incidents** and **Logs** tabs.

**Solution:** Use the following command on your PanelAlpha server:

```bash
docker compose -f /opt/panelalpha/app/docker-compose.yml exec api php artisan diagnostic-mode --app-id=INSTANCE_ID --disable
```

**Note:** Replace `INSTANCE_ID` with the actual ID of the instance for which you want to disable diagnostic mode.
