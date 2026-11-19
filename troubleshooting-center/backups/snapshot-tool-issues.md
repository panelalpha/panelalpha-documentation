---
title: Snapshot Tool Issues
description: Resolve PanelAlpha Snapshot Tool issues including installation errors, backup failures, restoration problems, and automation issues.
nav:
  group: Backup Issues
  order: 2
---


# Snapshot Tool Issues

This section covers common problems and solutions when using the PanelAlpha Snapshot Tool to create, manage, and restore backups.

## Installation & Setup Issues

### Permission Denied When Running Snapshot Commands

**Problem:** You receive a "Permission denied" error when running snapshot commands.

**Solution:** Always run the snapshot tool with `sudo`:

```bash
sudo /opt/panelalpha/pasnap.sh --snapshot
```

The tool requires root access to:
- Access Docker containers and volumes
- Read/write system configuration files
- Create database dumps
- Manage file permissions

### Setup Wizard Fails with "Docker Not Found" Error

**Problem:** The setup wizard fails because Docker is not found.

**Solution:** Ensure Docker is installed and running:

```bash
# Check if Docker is installed
docker --version

# Start Docker service if not running
sudo systemctl start docker
sudo systemctl enable docker

# Verify Docker is working
sudo docker ps
```

**System Requirements:**
- Docker version 20.10 or higher
- At least 3 GB free disk space

### Setup Fails with "Repository Initialization Error"

**Problem:** The setup fails with a repository initialization error.

**Solution:**
1. Check storage credentials in your configuration:
   ```bash
   sudo nano /opt/panelalpha/pasnap/.env-backup
   ```
2. Test the repository connection:
   ```bash
   sudo /opt/panelalpha/pasnap.sh --test-connection
   ```
3. Verify credentials for your storage type:
   - **S3**: Verify Access Key ID and Secret Access Key
   - **SFTP**: Check hostname, username, and SSH key permissions
   - **Local**: Ensure the target directory exists and has write permissions

## Backup Creation Issues

### Snapshot Creation Fails with "Database Connection Error"

**Problem:** Snapshot creation fails with a database connection error (for example, a message referencing `API_MYSQL_PASSWORD` or checking the database password in `.env`).

**Solution:**

1. Run the built-in database diagnostic:
   ```bash
   sudo /opt/panelalpha/pasnap.sh --verify-database
   ```

2. Verify PanelAlpha is running:
   ```bash
   cd /opt/panelalpha/app
   docker compose ps
   ```

3. Confirm the correct database password variable for your installation:

   | Installation | Environment file | Password variable | Database user | Container |
   |---|---|---|---|---|
   | Control Panel | `/opt/panelalpha/app/.env` | `API_MYSQL_PASSWORD` | `panelalpha` | `database-api` |
   | Engine | `/opt/panelalpha/engine/.env` or `.env-core` | `CORE_MYSQL_PASSWORD` | `core` | `database-core` |
   | Engine | `/opt/panelalpha/engine/.env` or `.env-core` | `USERS_MYSQL_ROOT_PASSWORD` | `root` | `database-users` |

   Control Panel example:
   ```bash
   grep API_MYSQL_PASSWORD /opt/panelalpha/app/.env
   ```

   You do **not** need to copy the password anywhere else. The Snapshot Tool reads it automatically from the PanelAlpha environment file.

4. Review snapshot logs for specific errors:
   ```bash
   sudo tail -f /var/log/pasnap.log
   ```

> [!NOTE]
> **Note:** MariaDB passwords are set when the database volume is first created. If `API_MYSQL_PASSWORD` was changed in `.env` later without updating the database user, snapshots may fail even when PanelAlpha still appears to work. Do **not** delete the database volume to fix this — that would cause data loss. Contact support if `--verify-database` reports a password mismatch.

### "Insufficient Disk Space" Error During Backup

**Problem:** Backup fails due to insufficient disk space.

**Solution:**
1. Check available space:
   ```bash
   df -h /var/tmp
   df -h /opt/panelalpha
   ```
2. Clean up old snapshots to free space:
   ```bash
   # List all snapshots
   sudo /opt/panelalpha/pasnap.sh --list-snapshots

   # Delete specific old snapshots
   sudo /opt/panelalpha/pasnap.sh --delete-snapshots <snapshot-id>
   ```

> [!WARNING]
> **Warning:** The `--delete-snapshots` flag permanently deletes backup snapshots. Verify the snapshot ID before running.

3. Configure cleanup in `.env-backup`:
   ```bash
   BACKUP_RETENTION_DAYS=30
   ```

### Backup Upload to S3/SFTP Fails

**Problem:** Backup upload to remote storage fails.

**Solution:**
1. Test network connectivity:
   ```bash
   sudo /opt/panelalpha/pasnap.sh --test-connection
   ```
2. Check credentials and permissions:
   - **S3**: Verify the bucket exists and the IAM user has write permissions
   - **SFTP**: Ensure the SSH key has correct permissions (`600`) and the user can write to the target directory
3. Review connection settings:
   ```bash
   sudo nano /opt/panelalpha/pasnap/.env-backup
   ```

## Restoration Issues

### Restore Fails with "Snapshot Not Found"

**Problem:** The restore process cannot find the specified snapshot.

**Solution:**
1. Verify the snapshot exists:
   ```bash
   sudo /opt/panelalpha/pasnap.sh --list-snapshots
   ```
2. Check the repository connection:
   ```bash
   sudo /opt/panelalpha/pasnap.sh --test-connection
   ```
3. Use the correct snapshot ID format:
   ```bash
   # Correct format (use actual ID from list-snapshots)
   sudo /opt/panelalpha/pasnap.sh --restore a1b2c3d4

   # Or restore latest
   sudo /opt/panelalpha/pasnap.sh --restore latest
   ```

### After Restoration, PanelAlpha Does Not Start Correctly

**Problem:** PanelAlpha does not start correctly after a restore.

**Solution:**
1. Check Docker container status:
   ```bash
   cd /opt/panelalpha/app
   docker compose ps
   docker compose logs
   ```
2. Verify database connectivity:
   ```bash
   docker compose exec database-api mysql -u panelalpha -p -e "SELECT 1;"
   ```
3. Restart all services:
   ```bash
   cd /opt/panelalpha/app
   docker compose down
   docker compose up -d
   ```
4. Check file permissions:
   ```bash
   sudo chown -R 1000:1000 /opt/panelalpha/app/storage
   ```

### Restored Instance Shows Wrong Domain/IP Addresses

**Problem:** The restored instance shows incorrect domain or IP addresses.

**Solution:** The restore process should automatically update IP addresses. If needed:
1. Update trusted hosts in PanelAlpha admin:
   - Go to **Admin Area → Configuration → General**
   - Update **Trusted Hosts** with the new server **domain names** (do not add the raw public IP)
2. If **admin login works by IP but not by hostname** (or Cloudflare Tunnel), configure Sanctum and trusted hosts on the server — see [Admin Area Login by Hostname or IP](../configuration/admin-area-login-hostname-or-ip.md).
3. Clear the application cache:
   ```bash
   cd /opt/panelalpha/app
   docker compose exec api php artisan cache:clear
   docker compose exec api php artisan config:clear
   ```

## Automation Issues

### Automatic Backups Are Not Running

**Problem:** Scheduled automatic backups are not running.

**Solution:**
1. Check cron job status:
   ```bash
   sudo /opt/panelalpha/pasnap.sh --cron status
   ```
2. Verify the cron service is running:
   ```bash
   sudo systemctl status cron
   ```
3. Check cron logs:
   ```bash
   sudo journalctl -u cron -f
   sudo tail -f /var/log/pasnap.log
   ```
4. Reinstall automation if needed:
   ```bash
   sudo /opt/panelalpha/pasnap.sh --cron remove
   sudo /opt/panelalpha/pasnap.sh --cron install
   ```

> [!NOTE]
> **Note:** Removing and reinstalling the cron schedule creates a window where automatic backups do not run. Perform this outside your backup window.

### Automated Backups Fail but Manual Backups Work

**Problem:** Automated backups fail while manual backups succeed.

**Solution:**
1. Check environment variables in the cron context:
   ```bash
   sudo crontab -l
   ```
2. Verify log file permissions:
   ```bash
   sudo ls -la /var/log/pasnap.log
   sudo chmod 644 /var/log/pasnap.log
   ```
3. Test with full paths in the cron job:
   ```bash
   sudo crontab -e
   ```

## Configuration Issues

### How to Change Backup Storage Location After Setup

**Problem:** You need to change the backup storage location after initial setup.

**Solution:**
1. Run the setup wizard again:
   ```bash
   sudo /opt/panelalpha/pasnap.sh --setup
   ```
2. Or manually edit the configuration:
   ```bash
   sudo nano /opt/panelalpha/pasnap/.env-backup
   ```
3. Test the new configuration:
   ```bash
   sudo /opt/panelalpha/pasnap.sh --test-connection
   ```

### How to Change Backup Schedule

**Problem:** You need to change the backup schedule.

**Solution:**
1. Edit the backup hour setting:
   ```bash
   sudo nano /opt/panelalpha/pasnap/.env-backup
   # Modify: BACKUP_HOUR=2 (for 2 AM)
   ```
2. Reinstall the cron job to apply changes:
   ```bash
   sudo /opt/panelalpha/pasnap.sh --cron remove
   sudo /opt/panelalpha/pasnap.sh --cron install
   ```

## Security & Access Issues

### "Access Denied" Errors When Accessing S3 Storage

**Problem:** You receive "Access denied" errors when accessing S3 storage.

**Solution:**
1. Verify IAM permissions for your S3 user:
   - `s3:ListBucket` on the bucket
   - `s3:GetObject` and `s3:PutObject` on bucket contents
   - `s3:DeleteObject` for cleanup operations
2. Check AWS credentials:
   ```bash
   sudo nano /opt/panelalpha/pasnap/.env-backup
   # Verify AWS_ACCESS_KEY_ID and AWS_SECRET_ACCESS_KEY
   ```
3. Test with AWS CLI (if available):
   ```bash
   aws s3 ls s3://your-bucket-name/ --region your-region
   ```

### SFTP Authentication Fails

**Problem:** SFTP authentication fails during backup operations.

**Solution:**
1. Check SSH key permissions:
   ```bash
   sudo chmod 600 /path/to/ssh/key
   sudo chown root:root /path/to/ssh/key
   ```
2. Test the SSH connection manually:
   ```bash
   ssh -i /path/to/ssh/key user@hostname
   ```
3. Verify the user has write permissions on the target directory.

## Performance Issues

### Backups Take Too Long to Complete

**Problem:** Backups take an excessive amount of time to complete.

**Solution:**
1. Check available resources:
   ```bash
   htop
   df -h
   iostat -x 1
   ```
2. Use incremental backups for large installations:
   - Modify the backup strategy in configuration
   - Keep fewer full backups and more incrementals
3. Optimize the storage location:
   - Use local storage for faster initial backups
   - Consider a faster network connection for remote storage

### System Becomes Slow During Backup Operations

**Problem:** The system slows down significantly during backups.

**Solution:**
1. Schedule backups during low-usage hours:
   ```bash
   # Edit backup hour in configuration
   sudo nano /opt/panelalpha/pasnap/.env-backup
   BACKUP_HOUR=3  # 3 AM
   ```
2. Monitor system resources during backup:
   ```bash
   sudo iotop
   sudo nethogs
   ```
3. Consider I/O limits if running on shared infrastructure.

## Monitoring & Maintenance

### How to Monitor Backup Success or Failure

**Problem:** You need to monitor whether backups succeed or fail.

**Solution:**
1. Check recent backup logs:
   ```bash
   sudo tail -f /var/log/pasnap.log
   ```
2. Set up log monitoring with your preferred tool.
3. Create notification scripts that parse log output.
4. Run regular health checks:
   ```bash
   sudo /opt/panelalpha/pasnap.sh --cron status
   sudo /opt/panelalpha/pasnap.sh --list-snapshots
   ```

## Getting Additional Help

If none of these solutions resolve your issue:

1. Collect comprehensive logs:
   ```bash
   sudo tar -czf snapshot-logs.tar.gz /var/log/pasnap.log /opt/panelalpha/pasnap/.env-backup
   ```

> [!WARNING]
> **Security note:** The archive may contain secrets (API keys, SFTP credentials) from `.env-backup`. Remove or redact sensitive values before sending to support.

2. Document the issue:
   - Exact error messages
   - Steps to reproduce
   - System information (`uname -a`, `docker --version`)
3. Contact support with detailed information for faster resolution.

**Prevention is better than cure:** Regularly test your backup and restoration process in a staging environment to ensure everything works before you need it in production.
