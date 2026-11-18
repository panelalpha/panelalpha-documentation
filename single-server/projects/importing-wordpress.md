---
title: "Importing a WordPress site"
description: "Importing copies a WordPress site from another host into a new project on this server, using your WordPress admin login or the migration plugin."
nav:
  group: Projects
  order: 4
---

# Importing a WordPress site

Importing copies an existing WordPress site from another host into a new PHP hosting project on this server. It uses your WordPress admin login or the migration plugin. **Importing over FTP or SFTP is not available.**

Before you start:

- The old site must be online and reachable on the internet, not on this server.
- The new project gets the domain of the source site, without `www.`. Make sure that domain is not already used by another project on this server.

## Importing a site

1. Go to **My Projects**, select **Create First Project** (or the create button) and choose **WordPress**.
2. Choose **Import Existing WordPress Project**.
3. Enter **Your Current WordPress Project URL**. The panel checks that it can reach the site.
   - WordPress found: select **Continue**.
   - The site answers, but WordPress is not on it: `WordPress could not be detected on this URL.` Select **Continue Anyway** to go on.
   - The check fails: the reason shows under the field (for example `Source WordPress host did not resolve`) and **Continue** stays unavailable until the URL is right.
4. Choose **Simple Method** (recommended).
5. Choose how to connect:
   - **Use WordPress admin login credentials**: enter the **URL**, **Username** and **Password** of a WordPress administrator.
   - **Install the plugin manually**: select **Download Plugin**, install and activate it in the WordPress admin of the old site, then select **Continue**.
6. Select **Continue** to start.

![Importing a WordPress site: Add New, WordPress, Import Existing WordPress Project, the URL https://old-site.example.com, Simple Method, then Use WordPress admin login credentials with the URL, Username and Password (hidden) and Continue, until The import process has been started!](../../assets/single-server/importing-wordpress-connect.gif)

The admin login method may not work, depending on the security settings and the host of your old site. If it fails, use the plugin.

If the old domain does not lead to the old site yet (for example during a move), open **Advanced** on the first step and enter the old server's IPv4 address in **IP Address**. The panel then reaches the old site at that address.

The panel shows `Congratulations!` and `The import process has been started!`. If you leave the page while the panel is still starting the import, the browser asks you to confirm with `The import is still in progress. If you leave this page, the import may be interrupted.`

## Following the import

The project card shows **Migration is in progress**, then **Import completed** or **Importing failed.** Open the migration log to see each step and its status.

- **Cancel Migration** stops it and removes what was entered.
- **Try Again** retries a failed import.

![The oldsite card in My Projects shows Migration is in progress with Show Logs and Cancel Migration, and Show Logs opens Migration Logs with the progress of the current step and the status of each step](../../assets/single-server/importing-wordpress-progress.gif)

## After the import

1. Open the site and check it.
2. If you used the plugin, remove it from the old site.
3. Point the domain to this server: [Domains and SSL](../looking-after-a-project/domains-and-ssl.md).

When the domain points here, the engine removes the preview address and requests a Let's Encrypt certificate. It checks every 15 minutes.

Something not working? Messages and fixes: [Troubleshooting: Projects](../troubleshooting/projects.md#importing-a-wordpress-site).
