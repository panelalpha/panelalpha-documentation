---
title: "WordPress Advanced pages"
description: "The Advanced tab of a WordPress site holds its themes, users, wp-config settings, automatic updates, security options and developer settings."
nav:
  group: Projects
  order: 4
---

# WordPress Advanced pages

Open a WordPress site and select the **Advanced** tab. A menu on the left lists the pages. This page explains the ones that belong to WordPress: **Security**, **Auto Updates**, **Developer Settings**, **System Configuration**, **Users** and **Installed Themes**. The other entries (**Project Details**, **Git**, **File Manager**, **Domains**, **FTP Accounts**, **SFTP Accounts**, **MySQL Databases**, **Cron Jobs** and **Logs**) work the same for every project and have their own pages: [Files and access](../looking-after-a-project/files-and-access.md), [Domains and SSL](../looking-after-a-project/domains-and-ssl.md), [Databases](../looking-after-a-project/databases.md), [Cron jobs and PHP](../looking-after-a-project/cron-and-php.md), [Statistics and logs](../looking-after-a-project/statistics-and-logs.md) and [Git](git.md).

The examples use the Stories site.

## Installed Themes

A theme is the design of the site. This page lists the themes installed on the site, as cards with a picture.

Each card shows the name of the theme and a line about it. The active theme is marked. The buttons are:

- **Use This Theme.** Makes the theme the design of the site. The card of the active theme says **Activated**.
- **Preview.** Shows what the theme looks like.
- **Update.** Appears when a newer version exists.
- **Remove.** Deletes the theme. You cannot remove the active theme. Switch to another one first.
- **Read more.** Opens the longer text about the theme.

**Add New** opens **Available Themes**, the themes from wordpress.org. A search field and a row of tags (for example a style or a feature) help you narrow the list, and the sort menu, set to **Most Rated** at first, changes the order. The arrow next to the title goes back to your installed themes. On a card in this list, **Install This Theme** adds the theme to the site, and you can then use it. **You install a theme by its wordpress.org name.** Installing from a URL is not available.

:docs-video{src="../../assets/single-server-edition/wordpress-themes-tab.webm" title="Advanced, Installed Themes of the Stories project: the installed themes Twenty Twenty-Four and Astra, Preview on a card, then Add New opens Available Themes, sorted by Most Rated, with the featured themes Kadence and Hello Elementor"}

## Users

This page lists the WordPress users of the site. These are the people who can sign in to the WordPress dashboard. It is not the list of people of your panel.

The table shows each user's **ID**, **Login**, display name, email and role. A role says what the user may do. WordPress has five: **administrator** (everything), **editor** (all posts and pages), **author** (own posts), **contributor** (writes posts but cannot publish them) and **subscriber** (can only read and keep a profile). The field **Enter search term** above the table finds a user.

Select **Create User** to add one. The window has these fields:

- **Username.** The name used to sign in. Up to 60 characters.
- **Email.** The address of the user. It must look like an email address.
- **Display Name.** The name shown on posts. Up to 60 characters.
- **Password.** At least 8 characters, with an upper case letter, a lower case letter, a number and a symbol. A short hint in the window lists the rules.
- **Role.** One of the five roles above.
- **Send email.** Tick it to send the new user an email about the account.

On each row, a button edits the user (the same fields, and an empty **Password** keeps the old one), **Reset Password** sets a new password after you confirm, and **Delete User** removes the user after you tick the confirmation box.

:docs-video{src="../../assets/single-server-edition/wordpress-users-page.webm" title="Advanced, Users of the Stories project: the user robin with ID, Login, Display Name, Email, Role, Registered and the action buttons, then Create User opens Create New User with Username, Email, Display Name, Password, Role and the Send email box"}

## System Configuration

This page holds the settings of the file `wp-config.php`. WordPress reads that file when it starts. **The page is for advanced users.** A wrong value can stop the site.

The table lists each setting with its **Name**, **Value** and **Type**. A type is either **Constant** or **Variable**. The database password and the secret keys are masked in the table. Select **Show** to read one, and **Hide** to cover it again. The panel may ask for your panel password first.

- **Create Configuration** adds a setting. Fill in **Name**, **Value** and **Type**. The name must be a valid PHP name: letters, numbers and underscores, and not starting with a number. The value has a size limit.
- The edit button on a row changes the value of the setting (**Edit Configuration Setting**).
- The delete button on a row removes the setting after you tick the confirmation (**Delete Configuration**).
- **Remove by name** removes a setting when you type its name (**Remove Configuration by Name**).

:docs-video{src="../../assets/single-server-edition/wordpress-system-configuration.webm" title="Advanced, System Configuration of the Stories project: the settings WP_MEMORY_LIMIT with the value 256M and WP_DEBUG with the value false, then Create Configuration opens Create Configuration with Name, Value and Type"}

## Auto Updates

Here you let the panel keep the site up to date by itself. Each option is a switch.

- **Auto Upgrade.** Updates WordPress itself when a new version comes out. When it is on, you choose how far it goes: **Update to any latest version available (Major as well as Minor)** or **Update to Minor versions only**. A minor update is a small fix for the same release. A major update brings a new release with new features, and it can change how the site looks or works.
- **Auto Upgrade Plugins.** Updates the plugins when a new version is available. If only some plugins update by themselves, the page says so and tells you to turn the switch on for all of them.
- **Auto Upgrade Themes.** Updates the themes to the latest version when one is available.

If WordPress itself blocks automatic updates with a setting in `wp-config.php`, the page names the setting. The panel then does not apply auto updates until you remove it.

## Security

- **SSL Certificate Applied.** Shows which certificate the domain uses, for example Let's Encrypt. **Manage** opens a window to change the certificate: [Domains and SSL](../looking-after-a-project/domains-and-ssl.md).
- **Force HTTPS Redirect.** When it is on, a visitor who opens `http://` goes to `https://` automatically. If other projects use the same domain, the panel lists them in a warning first, because the change applies to them too.
- **Disable Search Engine Indexing.** When it is on, the site asks search engines not to list it. Turn it on for a site that is not ready or is only a test.

:docs-video{src="../../assets/single-server-edition/wordpress-security-and-auto-updates.webm" title="Advanced, Security of the Stories project with SSL Certificate Applied, Force HTTPS Redirect and Disable Search Engine Indexing, then Auto Updates with the switch Auto Upgrade set to Update to Minor versions only and the switch Auto Upgrade Plugins"}

## Developer Settings

- **Debug Mode.** Shows application errors to visitors while they browse the site.
- **Maintenance Mode.** Shows visitors a maintenance notice instead of the site.
- **PHPMyAdmin.** A simple screen to manage the records of the database. **Log In to PHPMyAdmin** signs you in. Use it only if you know databases.
- **PHP Version.** Shows the current PHP version, for example `PHP 8.3`. **Manage** lets you choose another. The change applies to all projects on the domain.
- **PHP Settings.** **Manage** opens the PHP settings of the domain, such as the memory limit. The change applies to all projects on the domain. When the server does not allow changes, the card says so.

**Debug Mode and Maintenance Mode should be off on a live site.** Check both switches before the site goes live.

:docs-video{src="../../assets/single-server-edition/wordpress-developer-settings.webm" title="Advanced, Developer Settings of the Stories project: the switches Debug Mode and Maintenance Mode, then the cards PHPMyAdmin, PHP Version with PHP 8.3 and PHP Settings"}

PHP version and limits: [Cron jobs and PHP](../looking-after-a-project/cron-and-php.md).

Something not working? Messages and fixes: [Troubleshooting: Projects](../troubleshooting/projects.md#managing-wordpress).
