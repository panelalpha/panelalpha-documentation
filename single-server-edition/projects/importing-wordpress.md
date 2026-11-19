---
title: "Importing a WordPress site"
description: "Importing copies a WordPress site from another host into a new project on this server, using your WordPress admin login or the migration plugin."
nav:
  group: Projects
  order: 5
---

# Importing a WordPress site

Importing copies an existing WordPress site from another host into a new PHP hosting project on this server. It uses your WordPress admin login or the migration plugin. **Importing over FTP or SFTP is not available.**

Before you start:

- The old site must be online and reachable on the internet, not on this server.
- You need the address of the old site and either an administrator login of it or the right to install a plugin on it.
- The new project gets the domain of the source site, without `www.`. Make sure that domain is not already used by another project on this server.

## Importing a site

Each step below is one window. The progress dots at the bottom show how far you are.

1. Go to **My Projects**, select **Add New** (or **Create First Project**), choose **WordPress** and select **Continue**.
2. In **Add New Project**, choose **Import Existing WordPress Project** ("Import your site from another provider by filling out a simple form") and select **Continue**.
3. In **Import Existing WordPress Project**, enter **Your Current WordPress Project URL**, the address of the site you want to copy. The panel checks that it can reach the site while you type.
   - WordPress found: select **Continue**.
   - The site answers, but WordPress is not on it: `WordPress could not be detected on this URL.` Select **Continue Anyway** to go on.
   - The check fails: the reason shows under the field (for example `Source WordPress host did not resolve`) and **Continue** stays unavailable until the URL is right.
4. If the old domain does not lead to the old site yet (for example during a move), open **Advanced** on this window and enter the old server's IPv4 address in **IP Address**. The panel then reaches the old site at that address. Leave it empty otherwise.
5. Choose how to copy the site:
   - **Simple Method**, marked **Recommended**. It uses the migration plugin for WordPress. Choose this one.
   - **Advanced Method**, which asks for FTP or SFTP details. The panel shows it, but the server refuses it, so do not use it.
6. On the Simple Method window, choose how to connect:
   - **Use WordPress admin login credentials.** Enter the **URL**, the **Username** and the **Password** of a WordPress administrator of the old site. The panel does the rest on its own. This may not work, depending on the security settings and the host of your old site. If it fails, use the plugin.
   - **Install the plugin manually.** Select **Download Plugin** and install and activate it in the WordPress admin of the old site. Then come back and select **Continue**.
7. Select **Continue** to start.

:docs-video{src="../../assets/single-server-edition/importing-wordpress-connect.webm" title="Importing a WordPress site: Add New, WordPress, Import Existing WordPress Project, the URL stories.example.net, Simple Method, Use WordPress admin login credentials with the URL, Username and Password (hidden), Continue, until The import process has been started!"}

The new project gets the domain of the source site, without `www.`.

The panel shows `Congratulations!` and `The import process has been started!`, and tells you that the site will be imported under its domain. The import takes some time, and the panel sends a notification when it ends. If you leave the page while the panel is still starting the import, the browser asks you to confirm with `The import is still in progress. If you leave this page, the import may be interrupted.` Select **Return To Dashboard** to go back to the project list.

## Following the import

The project card shows **Migration is in progress** with a bar and the name of the current step, then **Import completed** or **Importing failed.** Select **Show Logs** on the card to open **Migration Logs**. The window lists each step with its status (not started, in progress, completed or failed), and shows the progress of the step that runs now. A step name is, for example, **Migrating Database** or **Migrating Files**.

- **Cancel Migration** stops it and removes what was entered.
- **Try Again** retries a failed import.

:docs-video{src="../../assets/single-server-edition/importing-wordpress-progress.webm" title="The stories.example.net card in My Projects shows Migration is in progress, and Show Logs opens Migration Logs with the progress of the current step, Migrating Database, and the status of each step"}

## After the import

1. Open the site and check it.
2. If you used the plugin, remove it from the old site.
3. Point the domain to this server: [Domains and SSL](../looking-after-a-project/domains-and-ssl.md).

When the domain points here, the engine removes the preview address and requests a Let's Encrypt certificate. It checks every 15 minutes.

Something not working? Messages and fixes: [Troubleshooting: Projects](../troubleshooting/projects.md#importing-a-wordpress-site).
