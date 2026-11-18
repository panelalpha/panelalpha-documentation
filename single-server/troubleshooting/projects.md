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

**"The server operator has protected this project, so it cannot be deleted, stopped or redeployed from the panel."** The person who runs the server marked the project as protected. Ask them to lift the protection.

**"The project is busy with another operation. Please try again later."** A deploy, backup or other task is running on the project. Wait for it to finish and try again: [Tasks](../the-panel/tasks.md).

More about this: [Projects](../projects/projects.md).

## Create a project

**"Invalid repository URL."** The address is not an HTTPS or SSH repository address. Copy it from your Git provider.

**"The repository was not found. Check the repository address and that the token can read it if the repository is private."** The address is wrong, or the repository is private and the token cannot read it.

**"The repository refused this token. Check the token and that it can read this repository."** The token is wrong or has no read access.

**"The host cannot be reached from the panel."** The panel cannot connect to that host. It refuses private addresses and the address of this server, so a repository on the same server cannot be used. Check the host name, or host the repository somewhere else.

**"Could not read the repository."** The panel could not open the repository and got no clear reason from the server. The host may be down or slow. Check the address and the token, then try again.

**"The repository did not accept the key. Check that you added it to this repository."** The deploy key is not in the repository yet. A **deploy key** is a key you add to the repository so the panel can read it. Add it in the repository's deploy key settings and select **Continue** again.

**PanelAlpha GO! is greyed out: "Connect this panel to PanelAlpha Hub first to browse and install apps."** The catalog needs the panel connected to PanelAlpha Hub. Select **Connect to PanelAlpha Hub** and connect in **Settings**: [Settings](../the-panel/settings.md#panelalpha-hub).

**"The catalog is unavailable"** The panel could not reach PanelAlpha GO!. Select **Try Again** in a few minutes. Creating a project from WordPress, Git or files still works.

**"The app's details could not be loaded from PanelAlpha Go. Try again in a moment."** PanelAlpha GO! did not answer for this app. Go back to the catalog and open the app again in a moment.

**"Add only one archive."** You added a folder or more than one file. Upload a single `.zip` or `.tar.gz`.

**"Allowed file types: .zip, .tar.gz"** The file has another type. Pack it as `.zip` or `.tar.gz`.

**"Domain is already in use."** A project on this server already has that domain. Choose another one.

**"Installation failed."** The install did not finish. Select **Try Again**. What you see depends on the kind of project:

- **WordPress project.** Its card in **My Projects** and its **Installation** card on **Project Details** read `Installation failed. Try Again`. Its progress window reads `Error!` and `Installation was unsuccessful. Please try again.`
- **Git or files project.** It has no **Installation** card. Its card reads `Deploy failed`. Its progress window reads `Error!` and `Something went wrong, please try again.`, with the deploy log below them. The deploy log holds the reason.

To read the deploy log, select **Show Logs** on the project's card in **My Projects**, or in the row's **Actions** menu in the table view.

On **Project Details**, a files project's **Source** card reads `Last deploy failed`. A Git project's **Source** card shows the repository, not the failure.

If the install fails again, see the task in [Tasks](../the-panel/tasks.md).

A project created from an archive cannot repeat that deploy, because the archive is deleted when its deploy ends. The panel asks for the archive again (see the next entry).

**Cancel Installation** deletes the project with its files and databases instead of trying again. Each of these opens the same confirmation:

- the card's **Cancel Installation** button
- the button behind the card's installation indicator (point at it)
- **Cancel Installation** in the table view's **Actions** menu

The delete runs only after you tick **Yes, I understand and want to cancel the installation.** in that confirmation.

**"This project deploys from an uploaded archive, and that archive is deleted when its deploy ends. Upload the archive again to retry."** **Try Again** has no archive left to deploy. Upload the archive again with **Upload new version** on the **Source** card of **Project Details**.

More about this: [Create a project](../projects/create-a-project.md).

## Managing WordPress

**"This project is not a WordPress installation."** The project is a container project. WordPress screens work only on a WordPress site.

**"Error: No matching entries found in 'wp-config.php'."** A setting holds a value too long to read back. **Advanced > System Configuration** says so where the list would be. To delete the setting, use **Remove by name**, next to **Create Configuration**. Then select **Retry** on the page, or load it again.

**"WordPress is already installed or being installed on this project."** An install is already running. Wait for it to finish.

**Maintenance Mode reads off, and Installation Details keeps the database values `wp-config.php` holds.** The panel cannot read the mode while WordPress cannot reach its database. So **Advanced > Developer Settings** shows Maintenance Mode off, and the **Installation** card keeps the installed version. A WordPress-typed project with no `wp-config.php` yet lists no database values there. Everything reads again once the database answers and `wp-config.php` exists.

**The Plugins tab, Installed Themes and both catalogues are empty.** The panel cannot read WordPress in the two states above. Listing plugins and themes, and searching wordpress.org for new ones, need the database and `wp-config.php`. The pages show no error message, only these texts:

- **Plugins** shows `No Installed Plugins Found`.
- **Advanced > Installed Themes** shows `No Installed Themes Found`.
- The catalogue of **New Plugins** and **Available Themes** shows `Start Searching For New Plugins` and `Start Searching For New Themes` instead of results.

Everything comes back once the database answers and `wp-config.php` exists.

**Advanced > System Configuration** is the exception. Its list is the `wp-config.php` file itself. When that file cannot be read, the page says why it lists nothing and keeps **Remove by name** and **Retry**. For a value too long to read back, the text is `Error: No matching entries found in 'wp-config.php'.`

More about this: [Managing WordPress](../projects/wordpress.md).

## Importing a WordPress site

**"Importing over FTP is not available. Use your WordPress admin login or the migration plugin."** The panel shows the **Advanced Method** (FTP and SFTP) form, but the server refuses it. Go back and choose **Simple Method**.

**"Source WordPress host did not resolve."** The panel could not reach that name. Check the name and that the old site is online, or fill in the old server's IPv4 address in **Advanced** (step 3 of [Importing a site](../projects/importing-wordpress.md#importing-a-site)).

**"No WordPress REST API at <address>"** WordPress did not answer at that address. Check the address of the old site and that its REST API is not blocked.

**"Source WordPress URL must lead to a public address: loopback, link-local, private, reserved, Docker network and this Engine's own addresses are refused."** The URL points at this server or at an address that is not public. Import over the old host's public domain or IP address. A site on this server cannot be imported into itself.

**"Cannot connect to WordPress. Please verify the URL or admin credentials. Alternatively, please install the plugin manually."** The URL or login is wrong, or the site cannot be reached. Check them, or use the plugin.

**"Using your WordPress admin credentials will not work (probably due to Cloudflare/firewall/WAF restrictions). Please install the plugin manually."** A firewall on the old site blocks the login. Install the plugin manually.

**"Migration plugin not found in your WordPress."** The plugin is not installed or not active on the old site. Install and activate it, then try again.

**"The migration plugin on your WordPress comes from another download. Download the plugin again, install and activate it, then try again."** Remove the old copy of the plugin and use the one from **Download Plugin**.

**"This site's domain is already used by a project on this server."** Another project already has that domain. Remove or rename that project first.

**"Cannot generate subdomain for imported project"** The source address is an IP address. Use the site's domain name as the URL.

**"Import preparation failed. Please try again."** The import did not start. Try again.

More about this: [Importing a WordPress site](../projects/importing-wordpress.md).

## Staging and preview

**"This project already has staging environment."** The project has a staging copy. Use it, or delete it and create a new one.

**"Cannot create staging project from staging project."** You cannot copy a staging copy. Create the copy from the live project.

**"A staging copy cannot use a panelalpha.online address: the PanelAlpha proxy assigns those names. Create the copy without a domain to get a free one."** The panel assigns the addresses in the free `panelalpha.online` zone itself, so the copy cannot be given one. Create the copy without a domain, or with an address on a domain of your own.

**"Subdomain must be a single name, without dots"** The panel adds the project's domain after the name, so enter only the name: `staging`, not `staging.example.com` (that would give `staging.example.com.example.com`).

**"The project is still being prepared. Please try again in a moment."** The project is not ready yet. Wait and try again.

**"The project has no live parent to push to."** The project is not a staging copy. **Push To Live** works only from a staging copy.

**"Could not compare database changes. You can still push files and database, but selective table changes are unavailable."** The panel could not read the tables. Push the full database, or push only files.

**"A preview tunnel is already being created for this project. Try again in a moment."** Another preview is being made. Wait a moment and select the button again.

More about this: [Staging and preview](../projects/staging-and-preview.md).
