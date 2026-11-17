---
title: Files, Logs and Services
description: Where PanelAlpha Engine 1.x keeps its files, logs and secrets, which containers it runs, and how to restart them over SSH.
nav:
  group: Server Management
  order: 7
---

# Files, Logs and Services

This page is for work on the Engine server over SSH: where things are, which containers run, and how to restart them. Run all commands as root.

## Files on the Server

The Engine is a Docker Compose project in `/opt/panelalpha/shared-hosting`.

| Path | What it is |
| --- | --- |
| `docker-compose.yml` | The main Compose file. It includes `docker-compose.yml-webserver`. |
| `docker-compose.yml-webserver` | A link to the Compose file of the active web server, for example `docker-compose.yml-litespeed`. Switching the web server changes this link. |
| `.env` | Settings and passwords, including `CORE_MYSQL_PASSWORD`, `USERS_MYSQL_ROOT_PASSWORD` and `CSF_UI_PASSWORD`. |
| `version` | The installed Engine version. |
| `config/` | Generated configuration, for example Exim and SFTP. |
| `webserver-config/` | Web server configuration and the [custom error pages](details.md#custom-error-pages). |
| `users/` | The Compose projects of hosting accounts (their PHP containers). |
| `logs/` | Engine and ModSecurity logs. |
| `webserver-logs/` | Access and error logs of the web server. |
| `scripts/` | Scripts used by the installer, updater and panel. |

Files of hosting accounts are not in this folder. Each account has a home folder in `/home/<username>`.

Do not edit generated files by hand unless support asks you to. The Engine writes them again when you change settings in the panel, rebuild an account or update.

## Containers

Show the containers and their state:

```bash
docker compose -f /opt/panelalpha/shared-hosting/docker-compose.yml ps
```

| Service | What it does |
| --- | --- |
| `core` | The Engine application and API. |
| `nginx` | Serves the Engine API on port 2011 and the CSF web interface for the panel. |
| `webserver` | The active web server on ports 80 and 443. |
| `database-core` | The Engine's own database. |
| `database-users` | MariaDB for customer databases. |
| `phpmyadmin-users` | phpMyAdmin for customer databases, opened from PanelAlpha. |
| `exim` | Sends outgoing mail. |
| `pure-ftpd` | FTP on port 21 and passive ports 30000–30009. |
| `sftp` | SFTP on port 2222. |
| `cron` | Scheduled jobs and log rotation. |
| `metrics` | Collects server metrics for the graphs and alerts. |
| `dns-proxy` | DNS for the web server containers. |
| `lighthouse` | Runs Lighthouse reports. |

Each hosting account also has its own containers for PHP. They are separate Compose projects that the Engine creates and rebuilds.

## Restarting Services

Restart one service:

```bash
docker compose -f /opt/panelalpha/shared-hosting/docker-compose.yml restart webserver
```

Start all services that are stopped:

```bash
docker compose -f /opt/panelalpha/shared-hosting/docker-compose.yml up -d
```

Use `restart` for one service when you can. Stopping the whole project takes all sites offline.

## Logs

| Path | What it holds |
| --- | --- |
| `/opt/panelalpha/log/engine-updates/` | One folder per update run, and `latest` for the last one. |
| `/opt/panelalpha/log/engine-updater_<date>.log` | The full log of each update. |
| `/opt/panelalpha/log/change-webserver/` | Logs of web server changes. |
| `/opt/panelalpha/log/monit_psdump_<date>.log` | Process lists saved by monit when memory ran out (see below). |
| `/opt/panelalpha/shared-hosting/logs/core/` | Logs of the Engine API. |
| `/opt/panelalpha/shared-hosting/logs/modsecurity/audit.log` | The [ModSecurity audit log](waf-modsecurity.md#audit-log-files). |
| `/opt/panelalpha/shared-hosting/webserver-logs/` | Web server access and error logs, one folder per web server. |

For the logs of one container, use:

```bash
docker compose -f /opt/panelalpha/shared-hosting/docker-compose.yml logs --tail 200 core
```

The outgoing mail log is inside the `exim` container. See [Outgoing Mail](outgoing-mail.md#emails-not-sending).

## Memory Protection (monit)

The installer sets up monit on the server. When memory usage goes above 91%, monit saves a list of processes to `/opt/panelalpha/log/monit_psdump_<date>.log` and restarts the two containers that use the most memory. This keeps the server running, but it also restarts whatever those containers serve.

If you find these log files, check them to see which site or service used the memory, then fix that site or add RAM.

The installer also sets the kernel to restart the server if it runs out of memory completely.

## Certificate for the Engine API

If the server has a public IP address, the installer and the updater request a Let's Encrypt certificate for that IP address. PanelAlpha uses it when it connects to the Engine API on port 2011.

## Uninstalling

The Engine package contains `uninstall.sh` in `/opt/panelalpha/shared-hosting`.

> [!WARNING]
> **Warning:** `uninstall.sh` stops all containers, deletes their Docker volumes, including the Engine and customer databases, and removes `/opt/panelalpha/shared-hosting`. Back up first. There is no undo. Account home folders in `/home` are not removed.
