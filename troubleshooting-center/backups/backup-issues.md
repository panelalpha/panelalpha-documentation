---
title: Backup Issues
description: Fix common PanelAlpha backup issues including failed automatic backups, restore errors, and download problems.
nav:
  group: Backup Issues
  order: 1
---


# Backup Issues

### “Queue Task Create Automatic Backup Failed”

**Problem:** Automatic backup tasks fail repeatedly.

**Solution:**
1. Check the disk quota on the hosting account and free up space if needed.
2. Verify remote backup credentials (S3/SFTP) and re-save the schedule to refresh them.
3. Confirm backup destinations exist and have write permissions.

### Restore Failed or Backup Archive Cannot Be Downloaded

**Problem:** Restore operations fail, or downloads return an error even though backup tasks were marked as completed.

**Solution:**
1. Validate remote backup storage settings and credentials in [Remote Backups](../../multi-server-edition/system-configuration/remote-backups.md).
2. Verify backup files exist in the selected storage and are readable by the configured account.
3. Check server task logs in **Admin Area → Logs → Tasks** for the exact failing step, then retry after fixing permissions or connectivity.
