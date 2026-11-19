---
title: PanelAlpha Snapshot Tool
description: Create encrypted backups, schedule automation, and restore or migrate PanelAlpha with the Snapshot Tool.
nav:
  group: System Configuration
  order: 15
  title: Snapshot Tool
---

# PanelAlpha Snapshot Tool

The PanelAlpha Snapshot Tool is a command-line utility for creating secure, encrypted backups (snapshots) of complete PanelAlpha installations. It supports comprehensive backup management, automated scheduling, and reliable restoration.

## Overview

The Snapshot Tool allows you to:

- Create complete backups of PanelAlpha (databases, files, configuration)
- Automatically create backups at scheduled times
- Restore the system on the same or new server
- Securely store backups in the cloud or locally

## What Gets Backed Up

### Databases
- **PanelAlpha API Database**: Complete MySQL dump with routines and triggers

### Docker Volumes
- `api-storage`: PanelAlpha application data
- `database-api-data`: MySQL data files
- `redis-data`: Redis cache and session data

### Configuration Files
- `docker-compose.yml`: Container orchestration configuration
- `.env` files: Environment variables and secrets
- `packages/`: Custom packages and extensions
- SSL certificates: Let's Encrypt certificates
- Nginx configurations: Web server settings

## Installation

Download and install the snapshot tool on your PanelAlpha server:

```bash
# Download the tool
wget -P /opt/panelalpha/ https://raw.githubusercontent.com/panelalpha/PanelAlpha-Snapshot-Tool/refs/heads/main/pasnap.sh

# Make it executable
chmod +x /opt/panelalpha/pasnap.sh

# Install required dependencies
sudo /opt/panelalpha/pasnap.sh --install
```

## Initial Configuration

Configure the backup storage location by running the setup wizard:

```bash
sudo /opt/panelalpha/pasnap.sh --setup
```

The setup wizard guides you through configuring:

- Storage backend type (local, S3, SFTP)
- Connection credentials
- Encryption password
- Backup retention settings

### Storage Options

#### Local Storage
- **Best for**: Development, testing, quick local backups
- **Repository format**: `local:/backup/panelalpha-snapshots`
- **Pros**: Fast, simple setup
- **Cons**: Single point of failure

#### SFTP Storage
- **Best for**: Remote server backups, existing SSH infrastructure
- **Repository format**: `sftp:backup-user@backup.example.com:/backups/panelalpha`
- **Pros**: Secure, widely supported
- **Requires**: SSH access to remote server

#### S3-Compatible Storage
- **Best for**: Production environments, scalable storage
- **Repository format**: `s3:s3.eu-west-1.amazonaws.com/my-bucket/panelalpha-snapshots`
- **Supports**: AWS S3, Hetzner Storage, MinIO, DigitalOcean Spaces
- **Pros**: Highly available, scalable, cost-effective

## Basic Operations

### Creating Snapshots

Create a manual backup:

```bash
sudo /opt/panelalpha/pasnap.sh --snapshot
```

### Listing Snapshots

View all available backups:

```bash
sudo /opt/panelalpha/pasnap.sh --list-snapshots
```

### Testing Connection

Verify repository connectivity:

```bash
sudo /opt/panelalpha/pasnap.sh --test-connection
```

Verify database credentials before creating snapshots:

```bash
sudo /opt/panelalpha/pasnap.sh --verify-database
```

## Backup Restoration

### Same Server Restoration

To restore PanelAlpha to a previous state on the same server:

1. **List available backups**:
   ```bash
   sudo /opt/panelalpha/pasnap.sh --list-snapshots
   ```

2. **Restore from a specific backup**:
   ```bash
   sudo /opt/panelalpha/pasnap.sh --restore a1b2c3d4
   ```

3. **Restore from the latest backup**:
   ```bash
   sudo /opt/panelalpha/pasnap.sh --restore latest
   ```

**Note:** Restoration replaces all current data. Ensure you have recent backups before proceeding.

### Migration to New Server

To migrate PanelAlpha to a new server:

**On the source server:**
1. Create a final backup:
   ```bash
   sudo /opt/panelalpha/pasnap.sh --snapshot
   ```
2. Note the backup ID from the command output.

**On the target server:**
1. Install PanelAlpha (basic installation only).
2. Copy and install the snapshot tool:
   ```bash
   sudo /opt/panelalpha/pasnap.sh --install
   ```
3. Configure the repository (use the same settings as the source server):
   ```bash
   sudo /opt/panelalpha/pasnap.sh --setup
   ```
4. Test the connection:
   ```bash
   sudo /opt/panelalpha/pasnap.sh --test-connection
   ```
5. Restore from backup:
   ```bash
   sudo /opt/panelalpha/pasnap.sh --restore a1b2c3d4
   ```

The restore process automatically:
- Updates database connection settings
- Adjusts server IP addresses in system settings
- Configures trusted hosts
- Recreates Docker volumes
- Restores SSL certificates

## Automated Backups

### Enable Automation

Install automatic daily backups:

```bash
sudo /opt/panelalpha/pasnap.sh --cron install
```

### Check Status

Monitor automation status:

```bash
sudo /opt/panelalpha/pasnap.sh --cron status
```

### Disable Automation

Remove automatic backups:

```bash
sudo /opt/panelalpha/pasnap.sh --cron remove
```

## Configuration File

Advanced configuration is stored in `/opt/panelalpha/pasnap/.env-backup`:

> [!NOTE]
> **Note:** Older installations may have a legacy file at `/opt/panelalpha/app/.env-backup`. The tool migrates it to `/opt/panelalpha/pasnap/.env-backup` automatically on first run.

```bash
# Repository settings
RESTIC_REPOSITORY="s3:s3.eu-west-1.amazonaws.com/my-bucket/panelalpha-snapshots"
RESTIC_PASSWORD="your-encryption-password"

# S3 credentials (if applicable)
AWS_ACCESS_KEY_ID="your-access-key"
AWS_SECRET_ACCESS_KEY="your-secret-key"

# Snapshot settings
BACKUP_RETENTION_DAYS=30
BACKUP_HOUR=2
BACKUP_TAG_PREFIX="panelalpha"

# System paths
LOG_FILE="/var/log/pasnap.log"
BACKUP_TEMP_DIR="/var/tmp"
RESTIC_CACHE_DIR="/var/cache/restic"
```

## Backup Management

### Delete Old Backups

Remove specific snapshots to free up storage space:

```bash
sudo /opt/panelalpha/pasnap.sh --delete-snapshots a1b2c3d4
```

### Check Disk Usage

Monitor storage consumption:

```bash
# Check available disk space
df -h

# Check backup repository size
sudo /opt/panelalpha/pasnap.sh --repository-stats
```

## Security Features

The Snapshot Tool implements enterprise-grade security:

- **Encryption**: All snapshots are encrypted using AES-256
- **Access Control**: Configuration files have restricted permissions (600)
- **Credential Management**: Cloud credentials are only exported during operations
- **Network Security**: HTTPS/TLS for all remote communications

## Monitoring and Logs

### Log Files

Monitor backup operations in `/var/log/pasnap.log`:

```bash
# View recent activity
sudo tail -f /var/log/pasnap.log

# Search for errors
sudo grep ERROR /var/log/pasnap.log
```

### Automatic Backup Monitoring

Check the status of scheduled backups:

```bash
sudo /opt/panelalpha/pasnap.sh --cron status
```

## Troubleshooting

### Common Issues

**Permission Denied**
```bash
# Ensure running with sudo
sudo /opt/panelalpha/pasnap.sh --snapshot
```

**Docker Not Running**
```bash
sudo systemctl start docker
sudo systemctl enable docker
```

**Repository Connection Failed**
```bash
# Test connection
sudo /opt/panelalpha/pasnap.sh --test-connection

# Verify credentials
sudo nano /opt/panelalpha/pasnap/.env-backup
```

**Insufficient Disk Space**
```bash
# Check available space
df -h /var/tmp

# Clean up old snapshots
sudo /opt/panelalpha/pasnap.sh --delete-snapshots old-snapshot-id
```

**Database Connection Errors**
1. Run database diagnostics: `sudo /opt/panelalpha/pasnap.sh --verify-database`
2. Verify PanelAlpha is running: `docker compose ps`
3. For Control Panel, confirm `API_MYSQL_PASSWORD` is set in `/opt/panelalpha/app/.env`
4. Review logs: `sudo tail -f /var/log/pasnap.log`

### Getting Help

If you encounter issues:

1. Check logs for specific error messages.
2. Run a connection test to validate repository access.
3. Contact PanelAlpha support.
