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

The **Installation** card on **Project Details** shows the WordPress version in large green numbers and whether an update is ready. Under it you see **Updated** when the site is current. A second column shows WooCommerce, the shop add-on for WordPress. It says **Not Installed** with an **Install Now** link, or its version. The wide button at the bottom reads **WordPress is up to date** and is disabled when there is nothing to update.

:docs-video{src="../../assets/single-server-edition/wordpress-installation-card.webm" title="The Installation card on Project Details of the Stories project: the WordPress version 6.4.0 with Updated under it, the WooCommerce line and the disabled WordPress is up to date button"}

When an update is ready, select **Update WordPress** on the card, or **Update** in the top bar or in the row's **Actions** menu. Pick the version, tick **Create Backup** if you want a backup first, and confirm. These buttons update WordPress itself, not plugins or themes. When only WooCommerce has an update, the card button reads **Update WooCommerce**.

## Plugins and themes

The **Plugins** tab opens on **Installed Plugins**. Each plugin is a card with its name and a line about it. The top edge of a card is green for an active plugin, grey for an inactive one and red when the plugin needs an update. The buttons on a card are:

- **Activate** turns an inactive plugin on. **Disable** turns an active plugin off.
- **Update** appears when a newer version exists. The tab name also shows a red mark with the number of plugins to update.
- **Remove** deletes the plugin from the site.

Above the cards, the two icons next to the title switch between cards and a table. **Name** sorts the list, and the field **Enter search term** filters the plugins you already have. **Add New** opens **New Plugins**. There you see the featured plugins from wordpress.org, and you can type a name in the search field to find others. Use **Install Plugin** on a card to add one. The arrow next to **New Plugins** takes you back.

:docs-video{src="../../assets/single-server-edition/wordpress-plugins-tab.webm" title="The Plugins tab of the Stories project: Installed Plugins with Akismet Anti-Spam, Classic Editor, Yoast SEO and Hello Dolly, then Add New and a search for contact form in New Plugins"}

**You install a plugin or theme by its wordpress.org name.** Installing from a URL is not available.

Themes, WordPress users, the settings in `wp-config.php`, automatic updates, security and developer settings are on the **Advanced** tab: [WordPress Advanced pages](wordpress-advanced.md).

To clear the cache, choose **Clear Cache** in the row's **Actions** menu in the table view, or **Cache** in the project's top bar, and confirm.

PHP version and limits: [Cron jobs and PHP](../looking-after-a-project/cron-and-php.md).
Domains and the certificate: [Domains and SSL](../looking-after-a-project/domains-and-ssl.md).

Something not working? Messages and fixes: [Troubleshooting: Projects](../troubleshooting/projects.md#managing-wordpress).
