---
title: "Projects"
description: "A project is one site or app in PanelAlpha Single Server Edition. The panel runs on a PanelAlpha Engine, and every project lives on that one server. Single..."
nav:
  group: Projects
  order: 1
---

# Projects

A project is one site or app in PanelAlpha Single Server Edition. The panel runs on a PanelAlpha Engine, and every project lives on that one server. **Single Server Free** has no project limit. In **Single Server Plus** and **Single Server Pro** the plan and the licence set it: [Editions and licence](../editions/editions-and-licence.md).

## The two kinds of project

The panel shows no label for the kind. You see it in what the project offers.

A **PHP hosting project** runs on the server's shared PHP. WordPress sites and other PHP sites are PHP hosting projects. A WordPress site is one: its project page has the WordPress screens, its card has a **Manage Site** button and a **WP Admin** button, and **WP Admin** also sits in the project's top bar. A PHP hosting project that is not WordPress has none of the WordPress screens and no **WP Admin**: its card has **Manage** and **Open Project**, and its project page has only **Project Details**, **Monitoring**, **Backups** and **Advanced**.

A **container project** runs in its own containers. A site created from Git or from files is a container project. Its card has a **Manage** button, and its project page has a **Containers** page. **Show Logs** opens the deploy log from its card in **My Projects** while it deploys or after a failed deploy, or from the row's **Actions** menu in the table view; **Advanced > Logs**, on the **Deploy Logs** tab, keeps the earlier deploys.

| You want to run | Create it from | Kind |
|---|---|---|
| A new or imported WordPress site | WordPress | PHP hosting project |
| An app from a Git repository | Git | Container project |
| An app from an archive of files | Files | Container project |

Create one: [Create a project](create-a-project.md).

## The project list

**My Projects** lists your projects as cards. Every card has a manage button (**Manage Site** on a WordPress card, **Manage** on others) and either a **WP Admin** button (WordPress) or an **Open Project** button. **Open Project** opens the site in a new tab when the project is on the panel's free `panelalpha.online` address; on a project with a domain of your own it opens the **Preview Site** window instead, whether or not that domain points to this server yet: [Staging and preview](staging-and-preview.md). In the table view each row ends with an **Actions** menu instead. What the menu offers depends on the kind of project and your edition: **Manage**, **WP Admin** (WordPress) or **Admin Area** (another app with its own admin login), **Show Logs**, **Push To Live**, **Staging**, **Update**, **Clear Cache**, **Share** and **Delete Project**.

<!-- TODO screenshot: assets/projects-list-actions-menu.png. Show My Projects in the table view with two rows (example.com and shop.example.com) and the Actions menu open on one of them. -->

**WP Admin** signs you in to the WordPress dashboard without a password: [Managing WordPress](wordpress.md).

**Share** is not shown in Single Server Free. Sharing a project with other people is a Single Server Pro feature: [Sharing projects](../editions/sharing-projects.md).

### Finding projects

Above the list:

- **Search**: type in the search field (`Enter search term`) to narrow the list.
- **Sort**: in the card view, choose **Name**, **Bandwidth**, **Storage** or **Newest**. In the table view, select the **Name**, **Bandwidth**, **Storage** or **Newest** header. A project the Engine has not measured yet has no bandwidth or storage figure, so it is listed after the projects that have one.
- **Filters**: select **Filters**, pick **Tags**, then **Apply**. **Clear Filter** removes them.
- **Grid or table**: the switch beside the heading changes the view.
- **Per page**: choose 8, 16, 32 or all projects per page.

To delete several projects at once, tick the box on each card or row. In the table view, the box in the header selects every project on the page. A bar at the bottom shows how many are selected, with **Select All**, **Clear Selection** and **Remove**. **Remove** opens the same delete dialog as **Delete Project**: [Deleting a project](#deleting-a-project).

## The project page

Select **Manage** to open a project. The tabs are **Project Details**, **Monitoring**, **Backups**, **Plugins** and **Advanced**. Some are shown only for a WordPress project or only when your account may use them.

**Project Details** shows the source of the project. A WordPress project has an **Installation** card. A Git project has a **Source** card with the repository. A files project has a **Source** card with its archive. Three more cards sit beside it:

- A thumbnail of the site, in the card beside **Installation** or **Source**. Hover over it for **Open Project** and **Refresh Thumbnail**. **Refresh Thumbnail** takes a new screenshot. Before the first one the card says `Preview not available.` and its **Why?** link explains why.
- **Loading Speed**: **Desktop** and **Mobile** scores, and a **Full Report** link to Google PageSpeed. This panel does not store the scores, so the card shows `Data Unavailable.`
- **Security**: **Automatic Backups**, **SSL** with **Manage**, and **Create Backup**.

The project name is at the top of the project page. On a WordPress project, select the pencil next to it, type a new name and confirm with the check mark or Enter. The name also becomes the WordPress site title. The title also opens a search field (`Search project...`) to jump to another project.

<!-- TODO screenshot: assets/projects-project-page-tabs.png. Show a project page with the tabs Project Details, Monitoring, Backups, Plugins and Advanced, and the Source or Installation card. -->

**Advanced** holds the rest, in a side list. It includes **Project Details**, which lists the plan, domain, URL, project name and creation date, and the pages for domains, databases, cron, files and logs. A WordPress project also shows its version there and its **Installation Details** on the same page: the directory and the database name, user, host and password. The **Git** page appears on every kind of project. The pages that belong to one kind appear only for that kind:

- **Environment variables** for projects that are not WordPress. A WordPress project keeps its variables in **System Configuration**.
- **Installed Themes**, **Users** and **Auto Updates** for WordPress.

<!-- TODO screenshot: assets/projects-advanced-menu.png. Show the Advanced tab with its side list (Domains, MySQL Databases, Cron Jobs, File Manager, Logs and the others) so the reader sees the page names. Use a WordPress project. -->

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

1. Open the project and go to **Advanced > Containers**.
2. Open the **Actions** menu at the top and choose **Start All**, **Stop All**, **Restart** or **Pull & Redeploy**. **Start All** starts the containers at once. The other three ask you to confirm first.

**Stop All and Restart make the site unavailable.** Stop All keeps it down until you select **Start All**. Restart takes it down until the containers run again.

Each container also has its own actions: [Statistics and logs](../looking-after-a-project/statistics-and-logs.md).

## Deleting a project

**Deleting a project erases its data on the server and cannot be undone.** Take a backup first if you may need it: [Backups](../looking-after-a-project/backups.md).

1. In **My Projects**, open the **Actions** menu of the project and choose **Delete Project**.
2. Tick **I understand that the project's files will be deleted**, **I understand that the project's databases will be deleted** and **Yes, I understand and want to proceed with the removal of the project.**
3. Select **Delete Project**.

The **Delete Project** button stays disabled until every box is ticked. The boxes are acknowledgements only: the panel always deletes the project with its files and databases, and the server has no option to keep them. For a WP Cloud site the dialog also warns that its manual backups must be deleted first.

A project that has a staging copy cannot be deleted. Delete the staging copy first: [Staging and preview](staging-and-preview.md).

## Troubleshooting

**"Cannot remove project with existing staging project."** The project has a staging copy. Delete the staging copy, then delete the project.

**"The server operator has protected this project, so it cannot be deleted, stopped or redeployed from the panel."** The person who runs the server marked the project as protected. Ask them to lift the protection.

**"The project is busy with another operation. Please try again later."** A deploy, backup or other task is running on the project. Wait for it to finish and try again: [Tasks](../the-panel/tasks.md).
