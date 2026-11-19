---
title: "Projects"
description: "Problems when you create, manage, import or copy a project, with what each message means and what to do."
nav:
  group: Troubleshooting
  order: 2
---

# Projects

Problems when you create, manage, import or copy a project, with what each message means and what to do.

## Projects

**"Cannot remove project with existing staging project."** The project has a staging copy. Delete the staging copy, then delete the project.

**"The server operator has protected this project, so it cannot be deleted, stopped or redeployed from the panel."** The project is protected, so the panel cannot delete, stop or redeploy it: [On the server](../getting-started/on-the-server.md#protected-projects).

**"The project is busy with another operation. Please try again later."** A deploy, backup or other task is running on the project. Wait for it to finish and try again: [Tasks](../the-panel/tasks.md).

More about this: [Projects](../projects/projects.md).

## Create a project

**"Invalid repository URL."** The address is not an HTTPS or SSH repository address. Copy it from your Git provider.

**"The repository was not found. Check the repository address and that the token can read it if the repository is private."**, **"The repository refused this token. Check the token and that it can read this repository."**, **"The host cannot be reached from the panel."** and **"Could not read the repository."** Check the address and the token. The token must be able to read the repository. The panel does not accept an address on this server, so a repository on the same server cannot be used.

**"The repository did not accept the key. Check that you added it to this repository."** The deploy key is not in the repository yet. A **deploy key** is a key you add to the repository so the panel can read it. Add it in the repository's deploy key settings and select **Continue** again.

**PanelAlpha GO! is greyed out: "Connect this panel to PanelAlpha Hub first to browse and install apps."** The catalog needs the panel connected to PanelAlpha Hub. Select **Connect to PanelAlpha Hub** and connect in **Settings**: [Settings](../the-panel/settings.md#panelalpha-hub).

**"The catalog is unavailable"** The panel could not reach PanelAlpha GO!. Select **Try Again** in a few minutes. Creating a project from WordPress, Git or files still works.

**"The app's details could not be loaded from PanelAlpha Go. Try again in a moment."** PanelAlpha GO! did not answer for this app. Go back to the catalog and open the app again in a moment.

**"Add only one archive."** You added a folder or more than one file. Upload a single `.zip` or `.tar.gz`.

**"Allowed file types: .zip, .tar.gz"** The file has another type. Pack it as `.zip` or `.tar.gz`.

**"Domain is already in use."** A project on this server already has that domain. Choose another one.

**"Installation failed."** The install did not finish. Select **Try Again**. A WordPress project shows `Installation failed. Try Again` on its card. A Git or files project shows `Deploy failed`, and its deploy log holds the reason: select **Show Logs** on the project's card in **My Projects**. If it fails again, see the task in [Tasks](../the-panel/tasks.md). **Cancel Installation** deletes the project with its files and databases instead of trying again, and asks you to confirm first.

**"This project deploys from an uploaded archive, and that archive is deleted when its deploy ends. Upload the archive again to retry."** **Try Again** has no archive left to deploy. Upload the archive again with **Upload new version** on the **Source** card of **Project Details**.

More about this: [Create a project](../projects/create-a-project.md).

## Managing WordPress

**"This project is not a WordPress installation."** The project is a container project. WordPress screens work only on a WordPress site.

**"Error: No matching entries found in 'wp-config.php'."** A setting holds a value too long to read back. **Advanced > System Configuration** says so where the list would be. To delete the setting, use **Remove by name**, next to **Create Configuration**. Then select **Retry** on the page, or load it again.

**"WordPress is already installed or being installed on this project."** An install is already running. Wait for it to finish.

**Maintenance Mode reads off, the Plugins tab, Installed Themes and the catalogues are empty, or the Installation card shows old values.** The panel cannot read WordPress while WordPress cannot reach its database, or while `wp-config.php` (the file that holds the site's settings) does not exist yet. The pages show no error, only texts such as `No Installed Plugins Found`. Everything reads again once the database answers and `wp-config.php` exists.

**Advanced > System Configuration** lists the `wp-config.php` file itself. When that file cannot be read, the page says why and keeps **Remove by name** and **Retry**.

More about this: [Managing WordPress](../projects/wordpress.md).

## Importing a WordPress site

**"Importing over FTP is not available. Use your WordPress admin login or the migration plugin."** The panel shows the **Advanced Method** (FTP and SFTP) form, but the server refuses it. Go back and choose **Simple Method**.

**"Source WordPress host did not resolve."** The panel could not reach that name. Check the name and that the old site is online, or fill in the old server's IPv4 address in **Advanced** (step 3 of [Importing a site](../projects/importing-wordpress.md#importing-a-site)).

**"No WordPress REST API at <address>"** WordPress did not answer at that address. The REST API is the part of WordPress the import talks to. Check the address of the old site and that nothing blocks it.

**"Source WordPress URL must lead to a public address: loopback, link-local, private, reserved, Docker network and this Engine's own addresses are refused."** The URL points at this server or at an address outside the public internet. Use the old site's public domain or IP address. A site on this server cannot be imported into itself.

**"Cannot connect to WordPress. Please verify the URL or admin credentials. Alternatively, please install the plugin manually."** or **"Using your WordPress admin credentials will not work (probably due to Cloudflare/firewall/WAF restrictions). Please install the plugin manually."** The URL or login is wrong, the site cannot be reached, or a firewall on the old site blocks the login. Check them, or install the migration plugin manually.

**"Migration plugin not found in your WordPress."** The plugin is not installed or not active on the old site. Install and activate it, then try again.

**"The migration plugin on your WordPress comes from another download. Download the plugin again, install and activate it, then try again."** Remove the old copy of the plugin and use the one from **Download Plugin**.

**"This site's domain is already used by a project on this server."** Another project already has that domain. Remove or rename that project first.

**"Cannot generate subdomain for imported project"** The source address is an IP address. Use the site's domain name as the URL.

**"Import preparation failed. Please try again."** The import did not start. Try again.

More about this: [Importing a WordPress site](../projects/importing-wordpress.md).

## Staging and preview

**"This project already has staging environment."** The project has a staging copy. Use it, or delete it and create a new one.

**"Cannot create staging project from staging project."** You cannot copy a staging copy. Create the copy from the live project.

**"A staging copy cannot use a panelalpha.online address: the PanelAlpha proxy assigns those names. Create the copy without a domain to get a free one."** The panel assigns `panelalpha.online` addresses itself. Create the copy without a domain, or with an address on a domain of your own.

**"Subdomain must be a single name, without dots"** The panel adds the project's domain after the name, so enter only the name: `staging`, not `staging.example.com` (that would give `staging.example.com.example.com`).

**"The project is still being prepared. Please try again in a moment."** The project is not ready yet. Wait and try again.

**"The project has no live parent to push to."** The project is not a staging copy. **Push To Live** works only from a staging copy.

**"Could not compare database changes. You can still push files and database, but selective table changes are unavailable."** The panel could not read the tables. Push the full database, or push only files.

**"A preview tunnel is already being created for this project. Try again in a moment."** Another preview link is being made. Wait a moment and select the button again.

More about this: [Staging and preview](../projects/staging-and-preview.md).
