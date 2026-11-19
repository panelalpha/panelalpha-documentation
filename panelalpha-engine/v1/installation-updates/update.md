---
title: Update
description: Update PanelAlpha Engine safely. Back up the server, run the updater, check the result and recover from a failed update.
nav:
  group: Installation & Updates
  order: 2
---

# Update

Check the PanelAlpha [changelog](https://panelalpha.com/changelog) with each new PanelAlpha release. It says when a release needs a newer Engine version.

You can update the Engine in two ways:

- From PanelAlpha: on the server's [Details](../server-management/details.md#updating-panelalpha-engine) tab, click **Update Now**.
- From the command line on the Engine server, as described below. Use this if the panel shows "This server is outdated", if the update from the panel fails, or if you want to watch the output.

## What happens during an update

The updater downloads the new version, stops all Engine containers and replaces the Engine files. It then builds and starts the containers again and rebuilds the web server configuration and file permissions for every account. Websites are unavailable for a short time while the containers restart, so update outside peak hours.

The updater keeps files you may have changed in `webserver-config` and the ModSecurity exclusions. It does not change site files or databases.

**The updater does not roll back on its own.** If it fails, the server can be left between two versions.

## Before you update

1. Back up the server. A snapshot from your hosting provider is the simplest option.
2. Read the [changelog](https://panelalpha.com/changelog).
3. Check that the server has a few GB of free disk space for the new Docker images.

## Update steps

1. Log in to the Engine server as root:
   ```bash
   ssh root@your-server-ip
   ```

2. Download the updater:
   ```bash
   wget -N -P /opt/panelalpha https://license.panelalpha.com/engine-updater.sh
   ```

3. Run it:
   ```bash
   bash /opt/panelalpha/engine-updater.sh
   ```

   The updater uses the licence key saved on the server. To use a different key, add `--key 'YOUR_LICENSE_KEY'`.

4. The updater shows whether a newer version is available and asks you to confirm. Type `y`.

### Updater options

| Option | What it does |
| --- | --- |
| `--key <key>` | Uses this licence key and saves it on the server. |
| `--version <version>` | Installs a given Engine version instead of the latest. |
| `--background` | Runs without questions and in the background. The panel uses this option. |
| `--force` | Runs even if the updater cannot detect the installed version. |
| `--debug` | Prints more output. |

## Post-update verification

1. In PanelAlpha, open **Configuration → Servers → Hosting Servers** and click **Manage** on the server. Check the version on the Details tab.
2. Open a few sites on the server.
3. Create a test instance.
4. Check that all containers are running:

   ```bash
   docker compose -f /opt/panelalpha/shared-hosting/docker-compose.yml ps
   ```

## Recovering from a failed update

1. Read the logs of the last run:
   - `/opt/panelalpha/log/engine-updates/latest/` holds `stdout`, `stderr` and `exit_code`, plus `from_version` and `to_version`. Older runs are in folders named by Unix time.
   - `/opt/panelalpha/log/engine-updater_<date>.log` is the full log of each run.
2. Fix the cause shown in the logs, for example a full disk or no access to `license.panelalpha.com`.
3. Run the updater again:

   ```bash
   bash /opt/panelalpha/engine-updater.sh
   ```

4. To go back to an earlier version, run the updater with that version:

   ```bash
   bash /opt/panelalpha/engine-updater.sh --version <previous-version>
   ```

   This installs the files of that version again. It does not restore data. If something is still broken, restore your backup.

Only one updater can run at a time. If you see "Another instance is already running.", wait for the other run to finish.

For other problems, see [Troubleshooting](../troubleshooting.md#update-fails).
