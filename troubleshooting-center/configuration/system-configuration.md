---
title: System Configuration
description: Resolve PanelAlpha system configuration issues including FTP connections, stuck tasks, password resets, firewalls, and email delivery.
nav:
  group: Configuration Issues
  order: 2
---


# System Configuration

### Could Not Retrieve Directory Listing with FTP

**Problem:** Connection issues with FTP.

**Solution:** Unlock ports 21 and 30000–30009 on the PanelAlpha Engine server.

### Deleting Domains vs. Deleting Instances

**Question:** Why is the domain not completely deleted when I remove an instance?

**Answer:** Removing an instance does not automatically delete the domain. The domain is removed only after deleting the associated service from the admin area, making the domain reusable.

### Stuck or Hanging Tasks in Queue

**Problem:** Some background tasks are stuck in progress for a long time or appear to hang.

**Solution:** Restart the `queue-worker` container to refresh queue processing:

```bash
docker compose -f /opt/panelalpha/app/docker-compose.yml restart queue-worker
```

This can help recover task processing when queue jobs stop progressing.

### Resetting Admin Password

**Question:** How can I reset the admin password?

**Answer:** If you have forgotten your administrator password, reset it through SSH:

> [!WARNING]
> **Warning:** Resetting the admin password overwrites current credentials. Coordinate with other administrators before proceeding.

1. **Access your server** via SSH with root privileges.
2. **Execute the password reset command:**
   ```bash
   docker compose -f /opt/panelalpha/app/docker-compose.yml exec -T api php artisan admin:reset-password EMAIL_ADDRESS
   ```
   Replace `EMAIL_ADDRESS` with the actual email address of the administrator account.
3. **Follow the terminal prompts** to complete the password reset process.
4. **Use the new password** to log into the PanelAlpha admin panel at `https://your-domain:8443`.

### Resetting Admin 2FA from Console

**Question:** How can I disable 2FA for an administrator who is locked out?

**Answer:** If an administrator cannot log in because of a lost authenticator device or another 2FA issue, you can disable 2FA from the server console without deleting the account or editing the database manually.

> [!WARNING]
> **Warning:** Disabling 2FA weakens account security. Re-enable 2FA immediately after regaining access.

1. **Access your server** via SSH with root privileges.
2. **Run the disable-2FA command:**
   ```bash
   docker compose -f /opt/panelalpha/app/docker-compose.yml exec -T api php artisan admin:disable-two-factor-authentication EMAIL_ADDRESS
   ```
3. **Confirm the operation** when prompted.
4. The command will clear the administrator's stored 2FA data.

### Template Instance Included in Pricing for PanelAlpha

**Question:** Does a WordPress template count as a website under the pricing plan?

**Answer:** Yes. Creating a WordPress template requires a functioning installation, and this installation is treated as a website under the pricing plan.

### Using Firewalls with PanelAlpha

**Question:** Can UFW or other firewalls be used with PanelAlpha, and which ports need to be open?

**Answer:** PanelAlpha is compatible with firewalls. Ensure that ports 80 and 443 are open for proper operation.

### Display Server Status

**Question:** How can I track the server status of PanelAlpha via WHMCS?

**Answer:** Create a status directory in `/opt/panelalpha/app/packages/client-area/dist/` and configure the Server Status Address in WHMCS at `https://{client_area_url}/status/`. For more information, refer to the official [WHMCS documentation](https://help.whmcs.com/m/troubleshooting/l/1601334-troubleshooting-status-monitoring).

### Expired Certificates

**Problem:** My SSL certificate has expired.

**Solution:** Set up a cron job to automatically renew the SSL certificate:

```bash
bash /opt/panelalpha/app/letsencrypt-renew.sh
```

Follow the instructions in [Renew Let's Encrypt Certificate](../../multi-server/getting-started/ssl-configuration.md#certificate-renewal).

### Emails Sent from WordPress Are Not Visible in the Queue

**Problem:** Emails from WordPress (for example, password resets) are marked as sent but do not show in the queue.

**Solution:** Check your inbox's spam folder to ensure WordPress emails are not being filtered.

### Loads of Processes Open and Lock the Server

**Problem:** The server freezes or becomes unresponsive due to a large number of open processes.

**Solution:** This issue may be caused by excessive RAM usage. Run `top` or `docker stats` to identify the process consuming resources, and ensure that the server's RAM limits are not being exceeded.

### Remote Backups in Scaleway S3 Configuration

See [Backup Issues](../backups/backup-issues.md) for restore/download failures.

### Mails Are Not Delivered Despite Successful Connection with Email Provider

**Problem:** The email service appears to be set up correctly with an external provider such as Mailjet, SendGrid, or Mailgun. The test connection from PanelAlpha is successful, but email messages are still not delivered to recipients.

**Solution:** Verify your email provider configuration:
- Confirm that the domain has been properly verified.
- Make sure the **Email Sender** is correctly set up; the "From" email in PanelAlpha must match the "Email Sender" in the email provider settings.

### Increasing phpMyAdmin Upload Limit in PanelAlpha Engine

**Problem:** The default upload limit for phpMyAdmin is too low for importing large database files.

**Solution:**

1. Connect to your PanelAlpha Engine server via SSH.
2. Create or edit the `docker-compose.override.yml` file in `/opt/panelalpha/shared-hosting/`:
   ```bash
   nano /opt/panelalpha/shared-hosting/docker-compose.override.yml
   ```
3. Add the following content:
   ```yaml
   services:
     phpmyadmin-users:
       environment:
         - UPLOAD_LIMIT=1G
   ```
4. Save the file and restart the services:
   ```bash
   docker compose -f /opt/panelalpha/shared-hosting/docker-compose.yml down phpmyadmin-users
   docker compose -f /opt/panelalpha/shared-hosting/docker-compose.yml up -d
   ```

After these changes, you should be able to upload larger database files through phpMyAdmin with the new 1 GB limit.
