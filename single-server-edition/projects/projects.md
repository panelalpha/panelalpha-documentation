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

## My Projects

**My Projects** is the first page after you sign in. It lists every project on your server. The list shows 8 projects at a time, newest first.

::docs-video{src="../../assets/single-server-edition/projects-list.webm" title="My Projects with the Bagisto and Immich cards: the address, the Production and Clients tags, the live mark and the Manage and Open Project buttons on a card, then the Filters, Newest and search controls and the grid and list switch"}
::

### The project card

Each project is one card. From top to bottom it shows:

- **A picture of the site.** Move the mouse over it to see the buttons. **Manage** opens the project page. **Open Project** opens the site (see [Open Project](#open-project)). A WordPress site has **Manage Site** and **WP Admin** instead. **WP Admin** signs you in to the WordPress dashboard without a password: [Managing WordPress](wordpress.md).
- **A small box in the top left corner of the picture.** Tick it to select the project: [Selecting several projects](#selecting-several-projects).
- **The name.** Select it to open the project page.
- **The address.** The lock beside it shows that the site has a working SSL certificate. Select the address to open the site in a new tab. A project on the free `panelalpha.online` address has an address like `immich-9b17.panelalpha.online`.
- **The tags.** See [Tags](#tags).
- **The word `live`.** It marks the live project. A project that has a staging copy shows a second mark for the copy: [Staging and preview](staging-and-preview.md).
- **A warning sign.** It appears only when something needs your attention. Hold the mouse over it to read what is wrong and to fix it.

While a project is being created or deployed, its card shows the progress instead of the picture. **Show Logs** on that card opens the deploy log: [Deploy logs](git.md#deploy-logs).

### Open Project

**Open Project** works in two ways:

- On a project with the free `panelalpha.online` address, it opens the site in a new tab.
- On a project with a domain of your own, it opens the **Preview Site** window, whether or not that domain points to this server yet: [Staging and preview](staging-and-preview.md#preview-link).

### Tags

A tag is a coloured word you put on projects to group them, for example `Production`, `Clients` or `Personal`. A project can have several tags. Tags belong to your account, so the same tag can be on many projects.

::docs-video{src="../../assets/single-server-edition/projects-tags.webm" title="Add tags on the Karakeep card opens the Assigned Tags window with No tags assigned, the Assign existing tag list is opened and Personal is chosen, and the tag appears on the card"}
::

1. On a card, select **Add tags**. If the project already has tags, select the round **+** button after them.
2. The **Assigned Tags** window shows the tags of this project. Open **Assign existing tag...** and pick a tag. It is added at once.
3. To make a new tag, select **Add New** in the same window. Type a name, pick a colour and select **Add**. Use **Cancel** to go back.
4. To take a tag off the project, select the **x** on its chip in the **Assigned Tags** window.
5. Close the window with the **x** at its top right.

A tag that is still on a project cannot be deleted. The list shows `Cannot be removed if currently in use.` beside its **Remove** button.

Select a tag on a card to show only the projects with that tag.

### Filters, sort and search

The three controls on the right of the heading change what the list shows.

::docs-video{src="../../assets/single-server-edition/projects-filter-by-tag.webm" title="Filters opens the Tags list, Production is ticked and Apply shows only the three Production projects, then Filters and Clear Filter show all projects again"}
::

**Filters** filters by tag.

1. Select **Filters**. The window lists your tags.
2. Pick one or more tags. They appear in **Selected tags**.
3. Select **Apply**. The list shows the projects that have at least one of the tags you picked.
4. The **Filters** button now shows how many tags you use, and a **Clear Filter** link appears under it. To see all projects again, select that link, or open **Filters** and select **Clear Filter**.

**Sort** is the box that says **Newest**. It sets the order of the cards:

- **Name** orders the projects by name.
- **Bandwidth** orders the projects by the data they send to visitors.
- **Storage** orders the projects by the disk space they use.
- **Newest** orders the projects by the day you created them, the last one first.

**Search** is the box that says **Enter search term**. Type part of a project name or address. The list shrinks while you type. Empty the box to see all projects.

::docs-video{src="../../assets/single-server-edition/projects-sort-search-pages.webm" title="The Newest box is changed to Name, grafana is typed in the search box and only Grafana is left, the search is cleared, then page 2 and Records Per Page 16 show more projects"}
::

### Pages

Under the list you find the page numbers on the left. Use the numbers and the arrows to move between pages. **Records Per Page** is on the right. Choose **8**, **16**, **32** or **all** to see more projects at once.

### Grid and table view

The two small buttons beside the heading switch the view. The first one shows cards (the grid). The second one shows a table.

The table has one row for each project. Its columns are **Name**, **Status**, **Visitors**, **Bandwidth**, **Storage**, **Tags** and **Newest**, then **Actions**. Select the arrows beside **Name**, **Bandwidth**, **Storage** or **Newest** to sort by that column. A narrow window shows each row as a stack, one value under the other.

The **Actions** menu at the end of a row (the three dots) holds the same actions as the card, and more:

- **Manage** opens the project page.
- **Staging**, **Push To Live** and **Push To Staging** work with the staging copy: [Staging and preview](staging-and-preview.md).
- **WP Admin**, **Update** and **Clear Cache** are for WordPress projects. **Update** updates WordPress and **Clear Cache** clears the cache of the site.
- **Delete Project** deletes the project: [Deleting a project](#deleting-a-project).

### Selecting several projects

Tick the box on each card or row. A bar appears at the bottom with **Remove**. It opens the same dialog as **Delete Project**: [Deleting a project](#deleting-a-project).

### Notifications

The bell at the top right shows messages about your projects, for example a finished deploy or a backup. A green dot on it means there are messages you have not read.

## The project page

Select **Manage** on a card, or the name of the project, to open its page.

::docs-video{src="../../assets/single-server-edition/projects-project-page.webm" title="Opening the Immich card shows its project page: the Open Project, Staging and Delete Project buttons and Change Domain are shown one after another, then the tabs Monitoring, Backups, Advanced and Project Details"}
::

### The top bar

The top of the page shows:

- **The arrow on the left** goes back to **My Projects**.
- **The name.** The small arrow beside it opens a search box, so you can jump to another project.
- **The address.** The two squares beside it copy the address.
- **Change Domain** opens the **Set Your Domain** window, where you give the project a domain of your own: [Domains and SSL](../looking-after-a-project/domains-and-ssl.md).
- **The tags**, with the round **+** button to add more: [Tags](#tags).
- **The `live` tab, and a `staging` tab** when the project has a staging copy. Select a tab to switch between them. The **+** button beside them creates a staging copy.

The buttons on the right:

- **Open Project** opens the site: [Open Project](#open-project). A WordPress site also has **WP Admin**, which signs you in to the WordPress dashboard.
- **Preview** appears on a project with a domain of your own and opens the **Preview Site** window: [Preview link](staging-and-preview.md#preview-link).
- **Staging** creates a staging copy. When the copy exists, **Push To Staging** takes its place on the live project, and **Push To Live** shows on the copy: [Staging and preview](staging-and-preview.md).
- **Update** and **Cache** appear on a WordPress project. **Update** updates WordPress. It is grey when nothing can be updated, and the tip says `There are no available updates.` **Cache** clears the cache of the site.
- **Delete Project** deletes the project: [Deleting a project](#deleting-a-project).

Under the top bar are the tabs **Project Details**, **Monitoring**, **Backups** and **Advanced**. A WordPress project also has **Plugins**.

### Renaming a project

You can rename a WordPress project. The name also becomes the WordPress site title.

1. Open the project and select the pencil next to the name.
2. Type the new name.
3. Select the check mark to save it, or the cross to leave without saving.

::docs-video{src="../../assets/single-server-edition/projects-rename-project.webm" title="On the Stories project page the pencil next to the name opens the name field, Stories Blog is typed, and the check mark and cross buttons appear; the cross leaves the name as it was"}
::

A project that is not WordPress has no pencil. Its name stays as it was when you created it.

### Project Details

**Project Details** is the first tab. It shows these cards:

- **A picture of the site.** Move the mouse over it to see two buttons. **Open Project** opens the site: [Open Project](#open-project). **Refresh Thumbnail** takes a new picture of the site, for example after you changed how it looks.

- **Source** (a Git or Files project) shows where the project comes from: the repository and branch with the last deployed commit, the database, and whether the project is `Up to date`. **Manage** beside the database opens the containers page. The button at the bottom is **Redeploy** for a Git project: [Redeploying](git.md#redeploying). A WordPress project has an **Installation** card instead.
- **Security** shows when the next automatic backup runs, with a **Disable** button, the SSL certificate with a **Manage** button, and **Create Backup**, which makes a backup now: [Backups](../looking-after-a-project/backups.md). On a server with no backup store, the backup line shows `Unavailable` and `No backup store is set up.` with a **Set up store** link to **Settings > Backups**.
- **Loading Speed** shows two scores from 0 to 100, one for a computer (**Desktop**) and one for a phone (**Mobile**). **Full Report** opens the complete report from Google PageSpeed.
- **Project Analytics** shows the visitors, the time they stay and the data sent, for the last week or month. Choose the period in the box at the right.

::docs-video{src="../../assets/single-server-edition/projects-source-card.webm" title="The Project Details tab of Immich: the Source card with the repository immich-app/immich and the PostgreSQL database, the Security card with Create Backup, the Loading Speed scores with Full Report, and Project Analytics below"}
::

To take a new picture of the site, hold the mouse over the picture and select **Refresh Thumbnail**.

::docs-video{src="../../assets/single-server-edition/projects-refresh-thumbnail.webm" title="On the Immich project page the mouse goes over the picture of the site, and Refresh Thumbnail takes a new picture"}
::

### Monitoring and Backups

**Monitoring** shows charts of visitors, countries and the length of the visits. **Backups** lists the backups of the project, lets you make a new one and restore an old one: [Backups](../looking-after-a-project/backups.md).

### Advanced

**Advanced** holds the rest, in a list on the left. Select an item to open its page.

::docs-video{src="../../assets/single-server-edition/projects-advanced-pages.webm" title="The Advanced tab of Immich with its list on the left, opening Domains, MySQL Databases, Cron Jobs, File Manager, Containers and Logs one after another"}
::

Every kind of project has pages for its details (which also show its usage and limits: [Usage and limits of a project](../looking-after-a-project/statistics-and-logs.md#usage-and-limits-of-a-project)), security, domains, databases, cron, files, logs and Git. A WordPress project also shows its version and **Installation Details** (the directory and the database name, user, host and password). Other projects have **Environment variables**. A WordPress project keeps its variables in **System Configuration**.

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

1. Open the project and select **Delete Project** at the top right. You can also open the **Actions** menu of the project in the table view and choose **Delete Project**.
2. The **Delete Project** window says `Any data stored on the server will be permanently erased` and `this action cannot be undone!` Under it is the name of the project. Tick the three boxes: one for the files, one for the databases, and `Yes, I understand and want to proceed with the removal of the project.`
3. Select **Delete Project**. The button stays pale until every box is ticked. Select **Cancel** if you changed your mind.

::docs-video{src="../../assets/single-server-edition/projects-delete-project.webm" title="The Delete Project window for Immich with its warning, the project name and three unticked boxes, the Delete Project button still pale, and Cancel closing it without deleting anything"}
::

A project that has a staging copy cannot be deleted. Delete the staging copy first: [Staging and preview](staging-and-preview.md).

Something not working? Messages and fixes: [Troubleshooting: Projects](../troubleshooting/projects.md#projects).
