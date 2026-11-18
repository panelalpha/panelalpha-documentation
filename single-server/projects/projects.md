---
title: "Projects"
description: "A project is one site or app on your server. It is either a PHP hosting project or a container project."
nav:
  group: Projects
  order: 1
---

# Projects

A project is one site or app on your server. The panel has no project limit.

## The two kinds of project

The panel shows no label for the kind. You see it in what the project offers.

A **PHP hosting project** runs on the server's shared PHP. WordPress sites and other PHP sites are PHP hosting projects. A WordPress site also gets the WordPress screens and a **WP Admin** button. A PHP site that is not WordPress has only **Project Details**, **Monitoring**, **Backups** and **Advanced**.

A **container project** runs in its own containers. A project created from Git, from files or from the PanelAlpha GO! catalog is a container project. Its project page has a **Containers** page.

| You want to run | Create it from | Kind |
|---|---|---|
| A new or imported WordPress site | WordPress | PHP hosting project |
| An app from a Git repository | Git | Container project |
| An app from an archive of files | Files | Container project |
| A ready open-source app | PanelAlpha GO! | Container project |

Create one: [Create a project](create-a-project.md).

## The project list

**My Projects** lists your projects as cards. Each card has a **Manage** button (**Manage Site** on a WordPress card) and either **WP Admin** or **Open Project**.

![My Projects with the Northwind Journal, memos and ntfy cards and their Manage Site, WP Admin, Manage and Open Project buttons, then the table view with the Actions menu of the Northwind Journal row open: Manage, WP Admin, Staging, Update, Clear Cache and Delete Project](../../assets/single-server/projects-list.gif)

**Open Project** works in two ways:

- On a project with the free `panelalpha.online` address, it opens the site in a new tab.
- On a project with a domain of your own, it opens the **Preview Site** window, whether or not that domain points to this server yet: [Staging and preview](staging-and-preview.md).

**WP Admin** signs you in to the WordPress dashboard without a password: [Managing WordPress](wordpress.md).

The grid and table switch beside the heading changes the view. In the table view, each row ends with an **Actions** menu with the same actions, plus **Show Logs**, **Push To Live** and **Clear Cache**. Use the search field to find a project, or **Filters** to filter by tag.

**Show Logs** opens the deploy log of a container project. You find it on the card while the project deploys or after a failed deploy. **Advanced > Logs**, on the **Deploy Logs** tab, keeps the earlier deploys.

To delete several projects at once, tick the box on each card or row, then select **Remove** in the bar at the bottom. It opens the same dialog as **Delete Project**: [Deleting a project](#deleting-a-project).

## The project page

Select **Manage** to open a project. The tabs are **Project Details**, **Monitoring**, **Backups**, **Plugins** and **Advanced**. **Plugins** is only for a WordPress project.

On a WordPress project, you can rename the project: select the pencil next to the name, type a new name and confirm. The name also becomes the WordPress site title.

![Manage Site on the Northwind Journal card opens the project page with the Installation card, then the tabs Monitoring, Backups, Plugins and Advanced one after another](../../assets/single-server/projects-project-page.gif)

### Project Details

**Project Details** shows where the project comes from: an **Installation** card for WordPress, or a **Source** card with the repository (Git) or the archive (files). Beside it are a thumbnail of the site, **Loading Speed** scores with a **Full Report** link to Google PageSpeed, and a **Security** card with **Automatic Backups**, **SSL** and **Create Backup**. On a server with no backup store, **Automatic Backups** shows `Unavailable` and `No backup store is set up.` with a **Set up store** link to **Settings > Backups**.

::docs-video{src="../../assets/single-server/projects-source-card.webm" title="The project page of Immich on the Site Details tab: the Source card with the repository, the database and Up to date with Manage, the Security card with Create Backup, the Loading Speed scores, and the Project Analytics charts below"}
::

### Advanced

**Advanced** holds the rest, in a side list.

![The Advanced tab of the Northwind Journal WordPress project with its side list, opening Domains, MySQL Databases, Cron Jobs, File Manager and Logs](../../assets/single-server/projects-advanced-pages.gif)

Every kind of project has pages for its details (which also show its usage and limits: [Usage and limits of a project](../looking-after-a-project/statistics-and-logs.md#usage-and-limits-of-a-project)), domains, databases, cron, files, logs and Git. A WordPress project also shows its version and **Installation Details** (the directory and the database name, user, host and password). Other projects have **Environment variables**. A WordPress project keeps its variables in **System Configuration**.

Where each page is described:

- WordPress pages: [Managing WordPress](wordpress.md).
- Git: [Git](git.md).
- Domains and certificates: [Domains and SSL](../looking-after-a-project/domains-and-ssl.md).
- Databases: [Databases](../looking-after-a-project/databases.md).
- Backups: [Backups](../looking-after-a-project/backups.md).
- Files, FTP and SFTP: [Files and access](../looking-after-a-project/files-and-access.md).
- Cron and PHP: [Cron jobs and PHP](../looking-after-a-project/cron-and-php.md).
- Usage and logs: [Statistics and logs](../looking-after-a-project/statistics-and-logs.md).

## Starting, stopping and restarting

You start, stop and restart **container projects** from **Advanced > Containers**. A PHP hosting project has no start or stop. It is always on while the server runs.

**Stop All and Restart make the site unavailable.** Stop All keeps it down until you select **Start All**. Restart takes it down until the containers run again.

1. Open the project and go to **Advanced > Containers**.
2. Open the **Actions** menu at the top and choose **Start All**, **Stop All**, **Restart** or **Pull & Redeploy**. **Start All** starts the containers at once. The other three ask you to confirm first.

Each container also has its own actions: [Statistics and logs](../looking-after-a-project/statistics-and-logs.md).

## Deleting a project

**Deleting a project erases its files and databases on the server and cannot be undone.** Take a backup first if you may need it: [Backups](../looking-after-a-project/backups.md).

1. In **My Projects**, open the **Actions** menu of the project and choose **Delete Project**.
2. Tick the three boxes that confirm you understand.
3. Select **Delete Project**.

A project that has a staging copy cannot be deleted. Delete the staging copy first: [Staging and preview](staging-and-preview.md).

Something not working? Messages and fixes: [Troubleshooting: Projects](../troubleshooting/projects.md#projects).
