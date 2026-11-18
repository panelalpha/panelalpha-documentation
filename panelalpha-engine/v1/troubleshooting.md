---
title: Troubleshooting
description: "Fix common PanelAlpha Engine 1.x problems: failed installs and updates, connection to PanelAlpha, containers, blocked requests and web server changes."
nav:
  group: Troubleshooting
  order: 1
---

# Troubleshooting

These fixes are for PanelAlpha Engine 1.x, the version used with PanelAlpha Single Server and Multi Server. For Engine 2.0+, see the [Troubleshooting Center](../../troubleshooting-center/servers/panelalpha-engine.md).

Run the commands on this page as root on the Engine server. For what the paths mean, see [Files, Logs and Services](server-management/files-and-logs.md).

## Installation fails

The installer prints the reason when it stops. These are the common messages:

| Message | What to do |
| --- | --- |
| `Please run as root!` | Log in as `root`, or run `sudo -i` first. |
| `Unsupported operating system` | Install Debian 12 or 13, or Ubuntu 22.04 or 24.04, and run the installer again. |
| `The license key is invalid` or `Invalid license status` | Check the key in the [client area](https://manage.panelalpha.com/clientarea.php?action=services) and that the licence is active. |
| `Cannot find package` | The server could not download the Engine. Check that it can reach `license.panelalpha.com` over HTTPS. |

Also check that:

- PanelAlpha is not installed on the same server.
- No other web server, FTP server or control panel already uses ports 80, 443, 21 or 2011.

To send the output to support, run the installer with `--debug` and save it to a file:

```bash
bash /opt/panelalpha/engine-installer.sh --key 'YOUR_LICENSE_KEY' --debug 2>&1 | tee engine-install.log
```

## Update fails

First read the logs of the last run in `/opt/panelalpha/log/engine-updates/latest/` and `/opt/panelalpha/log/engine-updater_<date>.log`. Then:

| Message | What to do |
| --- | --- |
| `Another instance is already running.` | An update is already running. Wait for it to finish. |
| `not compatible with background updater. Manual update required` | The server is too old to update from the panel. Update from the command line as described in [Update](installation-updates/update.md). |
| `Cannot detect license key` or `License Key is required.` | Run the updater with `--key 'YOUR_LICENSE_KEY'`. |
| `The license key is invalid` | Check the licence in the client area, or pass another key with `--key`. |
| `Cannot detect current version` | Run the updater with `--force`. |

If the update stopped halfway, fix the cause and run the updater again: [Recovering from a failed update](installation-updates/update.md#recovering-from-a-failed-update).

## PanelAlpha cannot connect to the Engine

**Problem:** **Test Connection** fails, or the server shows an error in PanelAlpha.

- Check that port 2011 is open in your hosting provider's firewall and that PanelAlpha can reach it. To test, run `curl -k https://<engine-ip>:2011/api` on the PanelAlpha server. Any HTTP response means the port is reachable.
- Check the **API URL** in the server settings. It is the value the installer printed, for example `https://203.0.113.10:2011/api`.
- If you lost the API token, create a new one with `api-tokens:create` and enter it in the server settings: [Artisan Commands](command-reference.md#api-tokens).
- Check that the Engine containers run (see "Containers not starting" below).

## "This server is outdated"

The panel shows this message when the server runs an Engine version older than 1.0.9-beta, or when it cannot read the version at all.

- Run `system:version` on the Engine server. If the version is older than 1.0.9-beta, update from the command line as described in [Update](installation-updates/update.md).
- If the version is newer, PanelAlpha probably cannot reach the Engine: [PanelAlpha cannot connect to the Engine](#panelalpha-cannot-connect-to-the-engine).

## Containers not starting

```bash
docker compose -f /opt/panelalpha/shared-hosting/docker-compose.yml ps
docker compose -f /opt/panelalpha/shared-hosting/docker-compose.yml logs --tail 200 core
df -h
```

- A full disk stops containers. Free space first, then run `docker compose -f /opt/panelalpha/shared-hosting/docker-compose.yml up -d`.
- If `webserver` keeps restarting, read its logs with `logs --tail 200 webserver`. A wrong manual change in the web server configuration is a common cause.

## Containers restart on their own

If you find `monit_psdump_*.log` files in `/opt/panelalpha/log/`, the server ran out of memory. Monit (the tool that watches the containers) then restarted the containers that used the most. Read the newest file to see which processes used the memory: [Memory protection](server-management/files-and-logs.md#memory-protection-monit).

## Web server change failed

On the Details tab, hover over **Change status** to read the error. The full logs are in `/opt/panelalpha/log/change-webserver/`.

For LiteSpeed, a failed or expired trial licence is a common cause. Enter your LiteSpeed serial number and try again.

## Site returns 403 after a form submit or upload

ModSecurity (the web application firewall) may be blocking the request. Check the [audit log](server-management/waf-modsecurity.md#audit-log-files) for the site's domain and the time of the request. The entry shows the ID of the rule that matched. Turn off that rule file in **Loaded Config Files**, or add an exclusion in **Custom Rules**.

To check whether ModSecurity is the cause, switch the mode to **Detection Only** for a moment and try again.

## Missing www on a domain

**Problem:** `example.com` works but `www.example.com` does not.

```bash
docker compose -f /opt/panelalpha/shared-hosting/docker-compose.yml exec -T core php artisan users:add-missing-www-domain-aliases --all
```

## Wrong file owner or permissions

**Problem:** WordPress cannot write files, or a site shows permission errors after files were copied as root.

```bash
docker compose -f /opt/panelalpha/shared-hosting/docker-compose.yml exec -T core php artisan users:fix-file-permissions --username=<username>
```

## Emails not sending

See [Outgoing Mail](server-management/outgoing-mail.md#common-issues).

## Contact support

If none of this helps, contact the [Support Center](https://manage.panelalpha.com/contact). Include the Engine version (`system:version`), what you did and the relevant log files.
