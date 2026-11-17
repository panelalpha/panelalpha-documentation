---
title: "Managing WordPress"
description: "A WordPress site in the panel is a PHP hosting project. This page covers what you do with it after it is installed. Create one first: Create a project."
nav:
  group: Projects
  order: 3
---

# Managing WordPress

A WordPress site in the panel is a PHP hosting project. This page covers what you do with it after it is installed. Create one first: [Create a project](create-a-project.md).

## Logging in to WordPress

On a project card, **WP Admin** signs you in to WordPress without a password, and the card has no **Open Project** button. **Open Project** sits in the project's top bar, where **WP Admin** also sits: on the panel's free `panelalpha.online` address it opens the site in a new tab, and on a project with a domain of your own it opens the **Preview Site** window instead, whether or not that domain points to this server yet ([Staging and preview](staging-and-preview.md)). In the table view the row's **Actions** menu has **WP Admin** and no **Open Project** either. A site installed with the full install form also offers **Log in to WordPress** when the install finishes.

1. In **My Projects**, select **WP Admin** on the card (in the table view it is in the row's **Actions** menu), or open the project and select **WP Admin** in the top bar.
2. A site with one administrator signs in at once: the dialog says `Logging in to WordPress` while it makes the link, then opens the dashboard in a new tab.
3. With more than one administrator, pick one in **Log In As** and select **Generate Login Link**. It makes a one-off link and opens it in a new tab; if your browser blocks the tab, the dialog offers a **Log in to WordPress** button for it. The link also works in another browser.
4. If the site's domain does not point to this server yet, the dialog warns `Warning! This domain may not be pointing to this server yet.` and offers **Log in with Temporary URL**, which works before the DNS is set, or **Log in directly**.

## Updating WordPress

**Installation** on **Project Details** shows the WordPress version and whether an update is ready. While the site is current, the card shows the version with `Updated` under it, its action button is disabled and reads `WordPress is up to date` (`Systems are up to date` when WooCommerce is installed), and the top bar's **Update** is disabled with the tooltip `There are no available updates.` There is no **Update WordPress** button on the card while there is nothing to install.

<!-- TODO screenshot: assets/wordpress-installation-card.png. Show the Installation card on Project Details while the site is current: the WordPress version with "Updated" under it and the disabled "WordPress is up to date" button. -->

When an update is ready, the card keeps the installed version and adds `<new version> is available` under it, its action button becomes **Update WordPress** and can be selected, and the top bar's **Update** is enabled and shows no tooltip.

1. When an update is ready, select **Update WordPress** on the **Installation** card, or **Update** in the top bar, or **Update** in the row's **Actions** menu in the table view.
2. Pick the version and tick **Create Backup** if you want a backup first.
3. Confirm.

**Update** in the row's **Actions** menu and **Update** in the top bar apply to the WordPress core only, and the row's entry answers `No updates available.` while the core is current. A WooCommerce update comes from the same card button: when only WooCommerce has an update, that button reads **Update WooCommerce**.

## Plugins and themes

The **Plugins** tab lists **Installed Plugins** and **New Plugins**. For each plugin you can choose **Install Plugin**, **Activate**, **Disable**, **Update** or **Remove**. Use **Add New** and the search field to find more.

<!-- TODO screenshot: assets/wordpress-plugins-tab.png. Show the Plugins tab with Installed Plugins and New Plugins, the Add New button and the search field. -->

**You install a plugin or theme by its wordpress.org name.** Installing from a URL is not available: `Installing from a URL is not available on PanelAlpha Engine: a plugin or theme is installed by its wordpress.org name.`

Themes are under **Advanced > Installed Themes**, which lists **Installed Themes** and **Available Themes**. Use **Use This Theme**, **Preview**, **Install This Theme**, **Update** or **Remove**. The active theme cannot be removed.

## Other WordPress pages in Advanced

- **Users**: WordPress users. **Create User**, **Edit User**, **Reset Password** and **Delete User**. **Create User** needs a username, an email address, a display name, a role and a password. The password needs at least 8 characters, with an upper case letter, a lower case letter, a number and a symbol.
- **System Configuration**: values of `wp-config.php`. Create, edit and delete a setting. A value can be at most 4094 bytes long, which is about 4000 characters; a character outside plain ASCII takes more than one byte (`ą` two, `€` three), so fewer of those fit, and the dialog refuses a longer value. A value that is too long cannot be read back: the page then says why it lists nothing, in place of the settings, and **Remove by name**, next to **Create Configuration**, deletes a setting by the name you type. The database user and password and the keys and salts are masked (`********`); select **Show** to read one, which asks for your panel password when you have not entered it recently. Every request is recorded in the project's **Logs**.
- **Auto Updates**: **Auto Upgrade**, **Auto Upgrade Plugins** and **Auto Upgrade Themes**. The panel does not read the WordPress constants that block updates, so a site that sets one, such as `AUTOMATIC_UPDATER_DISABLED`, shows the same three switches and no message, and WordPress applies no automatic update there whatever the switches say. The constants are in `wp-config.php`, which **System Configuration** lists.
- **Developer Settings**: **Debug Mode**, **Maintenance Mode**, **PHPMyAdmin** and **PHP Version**.
- **Security**: **Force HTTPS Redirect** and **Disable Search Engine Indexing**.

<!-- TODO screenshot: assets/wordpress-developer-settings.png. Show Advanced, Developer Settings, with the cards Debug Mode, Maintenance Mode, PHPMyAdmin and PHP Version. Keep the Advanced side list in view. -->

**Debug Mode and Maintenance Mode should be off on a live site.** **Advanced > Developer Settings** shows both switches: Debug Mode makes the site display application errors to its visitors, and Maintenance Mode shows them the maintenance notice instead of the site. Check both before the site goes live.

Clearing the cache: choose **Clear Cache** in the row's **Actions** menu in the table view, or **Cache** in the project's top bar, and confirm.

PHP version and limits: [Cron jobs and PHP](../looking-after-a-project/cron-and-php.md).
Domains and the certificate: [Domains and SSL](../looking-after-a-project/domains-and-ssl.md).

## Troubleshooting

**"This project is not a WordPress installation."** The project is a container project. WordPress screens work only on a WordPress site.

**"Error: No matching entries found in 'wp-config.php'."** A setting holds a value too long to read back. **Advanced > System Configuration** says so where the list would be; use **Remove by name**, next to **Create Configuration**, to delete it, then **Retry** on the page or load it again.

**"WordPress is already installed or being installed on this project."** An install is already running. Wait for it to finish.

**Maintenance Mode reads off; Installation Details keeps the database values `wp-config.php` holds.** While WordPress cannot reach its database the panel cannot read the mode, so **Advanced > Developer Settings** shows Maintenance Mode off, and the **Installation** card keeps the installed version. A WordPress-typed project with no `wp-config.php` yet lists no database values there at all. Everything reads again once the database answers and `wp-config.php` exists.

**The Plugins tab, Installed Themes and both catalogues are empty while the panel cannot read WordPress.** Listing plugins and themes, and searching wordpress.org for new ones, needs the database and `wp-config.php`, so in the same two states **Plugins** shows `No Installed Plugins Found`, **Advanced > Installed Themes** shows `No Installed Themes Found`, and the catalogue of **New Plugins** and **Available Themes** shows `Start Searching For New Plugins` and `Start Searching For New Themes` instead of results, all without an error message. **Advanced > System Configuration** is the exception: its list is the `wp-config.php` itself, so when that file cannot be read the page says why it lists nothing (for a value too long to read back, `Error: No matching entries found in 'wp-config.php'.`) and keeps **Remove by name** and **Retry**. Everything comes back once the database answers and `wp-config.php` exists.
