---
title: "Managing WordPress"
description: "You log in to a WordPress site, update it, manage its plugins and themes and change its settings from the panel."
nav:
  group: Projects
  order: 3
---

# Managing WordPress

A WordPress site in the panel is a PHP hosting project. You log in to it, update it, manage its plugins and themes and change its settings from the project page. Create one first: [Create a project](create-a-project.md).

## Logging in to WordPress

**WP Admin** signs you in to WordPress without a password.

1. In **My Projects**, select **WP Admin** on the card (in the table view it is in the row's **Actions** menu), or open the project and select **WP Admin** in the top bar.
2. A site with one administrator signs in at once and opens the dashboard in a new tab.
3. With more than one administrator, pick one in **Log In As** and select **Generate Login Link**. The link works once. If your browser blocks the new tab, select **Log in to WordPress** in the dialog.
4. If the site's domain does not point to this server yet, the dialog warns `Warning! This domain may not be pointing to this server yet.` Select **Log in with Temporary URL**, which works before the DNS is set, or **Log in directly**.

### Opening the site

**Open Project** sits in the project's top bar, next to **WP Admin**. It behaves as described in [Projects](projects.md#the-project-list).

## Updating WordPress

The **Installation** card on **Project Details** shows the WordPress version and whether an update is ready.

:docs-video{src="../../assets/single-server/wordpress-installation-card.webm" title="The Installation card on Project Details of the Northwind Journal project: WordPress 7.1.4 with Updated under it and the disabled WordPress is up to date button"}

When an update is ready, select **Update WordPress** on the card, or **Update** in the top bar or in the row's **Actions** menu. Pick the version, tick **Create Backup** if you want a backup first, and confirm. These buttons update WordPress itself, not plugins or themes. When only WooCommerce has an update, the card button reads **Update WooCommerce**.

## Plugins and themes

The **Plugins** tab lists **Installed Plugins** and **New Plugins**. For each plugin you can choose **Install Plugin**, **Activate**, **Disable**, **Update** or **Remove**. Use **Add New** and the search field to find more.

:docs-video{src="../../assets/single-server/wordpress-plugins-tab.webm" title="The Plugins tab of the Northwind Journal project: Installed Plugins, then Add New and a search for contact form in New Plugins"}

**You install a plugin or theme by its wordpress.org name.** Installing from a URL is not available.

Themes are under **Advanced > Installed Themes**. Use **Use This Theme**, **Preview**, **Install This Theme**, **Update** or **Remove**. The active theme cannot be removed.

## Other WordPress pages in Advanced

- **Users**: create, edit, reset the password of and delete WordPress users. A new password needs at least 8 characters, with an upper case letter, a lower case letter, a number and a symbol.
- **System Configuration**: the settings in `wp-config.php`. It is for advanced users. You can create, edit and delete a setting. The database password and the keys are masked. Select **Show** to read one. The panel may ask for your panel password.
- **Auto Updates**: **Auto Upgrade**, **Auto Upgrade Plugins** and **Auto Upgrade Themes**.
- **Developer Settings**: **Debug Mode**, **Maintenance Mode**, **PHPMyAdmin** and **PHP Version**.
- **Security**: **Force HTTPS Redirect** and **Disable Search Engine Indexing**.

:docs-video{src="../../assets/single-server/wordpress-developer-settings.webm" title="Advanced > Developer Settings of the Northwind Journal project with the cards Debug Mode, Maintenance Mode, PHPMyAdmin and PHP Version"}

**Debug Mode and Maintenance Mode should be off on a live site.** Debug Mode shows application errors to visitors. Maintenance Mode shows them the maintenance notice instead of the site. Check both switches in **Advanced > Developer Settings** before the site goes live.

To clear the cache, choose **Clear Cache** in the row's **Actions** menu in the table view, or **Cache** in the project's top bar, and confirm.

PHP version and limits: [Cron jobs and PHP](../looking-after-a-project/cron-and-php.md).
Domains and the certificate: [Domains and SSL](../looking-after-a-project/domains-and-ssl.md).

Something not working? Messages and fixes: [Troubleshooting: Projects](../troubleshooting/projects.md#managing-wordpress).
