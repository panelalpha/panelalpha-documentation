---
title: PanelAlpha Snapshot Tool
description: "Take encrypted snapshots of a whole PanelAlpha server and restore them on the same server or a new one."
nav:
  group: System Configuration
  order: 15
  title: Snapshot Tool
---

# PanelAlpha Snapshot Tool

The PanelAlpha Snapshot Tool is a script, `pasnap.sh`, that saves everything a PanelAlpha server needs to come back after a failure: the database, Docker volumes, configuration files and certificates. Each saved copy is a **snapshot**. You run the script as `root` on the server.

Snapshots go to a **repository**, the place they are stored: a folder on the server, another server over SFTP, or an S3 bucket. The tool uses [Restic](https://restic.net), so every snapshot is encrypted with your password before it is written.

The Snapshot Tool recovers the whole installation. To restore one WordPress site, use the instance backups in the Admin Area or the Client Area.

The source code and the full reference are on GitHub: [PanelAlpha Snapshot Tool](https://github.com/panelalpha/PanelAlpha-Snapshot-Tool).

## What a snapshot contains

On a Multi Server installation (`/opt/panelalpha/app`) a snapshot contains:

- The panel database (`panelalpha` in the `database-api` container)
- The `api-storage`, `database-api-data` and `redis-data` volumes
- `.env`, `.env-api`, `docker-compose.yml` and the `version` file
- `packages/`, the panel code, including `vendor/`
- Certificates: `crt/`, `/etc/letsencrypt` and any other file that `SSL_CERT_FULLCHAIN` or `SSL_CERT_PRIVKEY` in `.env` points to
- The jobs in the `root` crontab that run PanelAlpha scripts, for example `letsencrypt-renew.sh`
- On panels older than 1.8.5: the Matomo database and the `matomo` volume

The tool takes a snapshot while the panel runs. It does not stop any service.

## Installing

```bash
wget -O /opt/panelalpha/pasnap.sh https://raw.githubusercontent.com/panelalpha/PanelAlpha-Snapshot-Tool/main/pasnap.sh
chmod +x /opt/panelalpha/pasnap.sh
sudo /opt/panelalpha/pasnap.sh --install
```

`--install` installs `restic`, `jq` and `rsync` with `apt` and checks that Docker runs. The server needs Debian or Ubuntu, Docker with the `docker compose` plugin and at least 3 GB free in `/var/tmp`.

## Setting up the repository

```bash
sudo /opt/panelalpha/pasnap.sh --setup
```

The setup asks for:

- The repository type and its details
- The encryption password
- How many days to keep snapshots (30 by default)
- The hour of the daily snapshot (2 by default)

It then connects to the repository and checks that it can log in to the panel database.

| Type | Example repository | Good for |
|:---|:---|:---|
| `local` | `/backup/panelalpha` | Tests. The snapshots are lost with the server. |
| `sftp` | `sftp:backup@backup.example.com:/backups/panelalpha` | Another server you reach over SSH as `root`, with a key and no password |
| `s3` | `s3:s3.eu-west-1.amazonaws.com/my-bucket/pasnap` | AWS S3, Hetzner, MinIO, DigitalOcean Spaces |

> [!IMPORTANT]
> **Without the encryption password, no snapshot can be restored.** Nobody, PanelAlpha included, can open a snapshot without it. Keep it outside the server, in a password manager or on paper.

## Taking a snapshot

```bash
sudo /opt/panelalpha/pasnap.sh --snapshot
```

At the end the tool prints `SUCCESS` and the snapshot ID, for example `a1b2c3d4`.

To take a snapshot every day at the hour from the setup:

```bash
sudo /opt/panelalpha/pasnap.sh --cron install
```

After each snapshot the tool keeps one snapshot per day for the number of days in `BACKUP_RETENTION_DAYS` and deletes the older ones of this server.

## Restoring

```bash
sudo /opt/panelalpha/pasnap.sh --list-snapshots
sudo /opt/panelalpha/pasnap.sh --restore a1b2c3d4
```

`--restore latest` picks the newest snapshot of this server.

> [!WARNING]
> **A restore replaces the panel's current data.** Anything created after the snapshot was taken is gone. The tool asks `Continue with restore? (yes/no)` and does nothing until you type `yes`.

During a restore the tool:

1. Stops the panel's containers.
2. Removes the database volume and imports the database from the snapshot.
3. Puts back the `api-storage` volume, the configuration files, `packages/` and the certificates.
4. Starts the panel with an empty Redis. Jobs that were waiting in the queue when the snapshot was taken have already run since then, so they are not run again.
5. Writes this server's IP address to the `host_ip_address` setting.
6. Adds the PanelAlpha jobs from the old `root` crontab that this server does not have.

The tool asks once more before it goes on when the snapshot comes from another panel version than the one installed. The restored database would not match the installed code.

**Trusted Hosts** stay as they were in the snapshot. If `SANCTUM_STATEFUL_DOMAINS` in `.env-api` lists hosts, the tool adds this server's hostname and IP address to it. When it is empty, the default, it stays empty.

If a certificate file named in `.env` is still missing after the restore, the tool names it. nginx does not start until that file is in place.

## Moving to a new server

1. On the old server, take a fresh snapshot and note its ID.
2. On the new server, install PanelAlpha **in the same version** as the old one.
3. Install the Snapshot Tool and run `--setup` with the same repository and the same encryption password.
4. Check that the snapshot is listed:
   ```bash
   sudo /opt/panelalpha/pasnap.sh --list-snapshots
   ```
5. Restore it:
   ```bash
   sudo /opt/panelalpha/pasnap.sh --restore a1b2c3d4
   ```
6. Point your domains at the new server.
7. Schedule the daily snapshot again with `--cron install`. The tool does not copy its own schedule, because the script may sit in another place on the new server.

To bring back an older panel version, install that version first, restore, then update the panel as usual.

## Commands

| Command | What it does |
|:---|:---|
| `--install` | Installs `restic`, `jq` and `rsync` |
| `--setup` | Asks for the repository and password, saves them |
| `--quickstart` | `--install`, `--setup` and `--snapshot` in one go |
| `--snapshot` | Takes a snapshot |
| `--snapshot-bg` | Takes a snapshot in the background, so you can close the terminal |
| `--list-snapshots` | Lists the snapshots in the repository |
| `--restore <id>` | Restores a snapshot, or the newest one with `latest` |
| `--delete-snapshots <id>` | Deletes one snapshot |
| `--test-connection` | Checks that the repository answers |
| `--verify-database` | Checks that the tool can log in to the panel database |
| `--cron install` | Adds the daily snapshot to the `root` crontab |
| `--cron status` | Shows the daily snapshot and the last lines of the log |
| `--cron remove` | Removes the daily snapshot |
| `--update` | Updates the script to the newest version |
| `--version` | Shows the version and the installation it found |

## Settings file

The setup saves its answers in `/opt/panelalpha/pasnap/.env-backup`, readable only by `root`:

```bash
RESTIC_REPOSITORY="s3:s3.eu-west-1.amazonaws.com/my-bucket/pasnap"
RESTIC_PASSWORD="your-encryption-password"
AWS_ACCESS_KEY_ID="your-access-key"
AWS_SECRET_ACCESS_KEY="your-secret-key"
BACKUP_RETENTION_DAYS=30
BACKUP_HOUR=2
LOG_FILE="/var/log/pasnap.log"
BACKUP_TEMP_DIR="/var/tmp"
RESTIC_CACHE_DIR="/var/cache/restic"
```

Older versions kept this file in `/opt/panelalpha/app/.env-backup`. The tool moves it on its first run.

## Updates

Once a day the script checks GitHub for a newer version. When it runs from cron, it updates itself. In a terminal, it asks first. Set `PASNAP_DISABLE_AUTO_UPDATE=1` to stop updates from cron, or `PASNAP_SKIP_UPDATE_CHECK=1` to stop the check.

## Logs

Every run writes to `/var/log/pasnap.log`. When a database dump or import fails, the log has the last lines of the error from MariaDB.

```bash
sudo tail -n 50 /var/log/pasnap.log
```

Known problems and fixes: [Snapshot Tool Issues](../../troubleshooting-center/backups/snapshot-tool-issues.md).
