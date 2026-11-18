---
title: "Staging and preview"
description: "A staging copy is a second copy of a project for trying changes. A preview link shows a site on a temporary address before its domain points here."
nav:
  group: Projects
  order: 6
---

# Staging and preview

A staging copy is a second copy of a project where you can try changes before they reach the live site. A preview link shows a site on a temporary address before its domain points to this server.

## Creating a staging copy

A project can have one staging copy. A staging copy cannot have its own.

1. Open the project and select **Staging** in the top bar.
2. Enter a **Subdomain** for the copy. The panel adds the project's own domain after it, so on `example.com` the name `staging` gives `staging.example.com`. Enter only the name, without dots.
3. For a WordPress site, you can tick **Skip Uploads Directory** to leave out `wp-content/uploads`. This saves space and time.
4. Select **Create Staging**.

A site on the panel's free `panelalpha.online` address does not ask for a name. The copy gets its own free address there.

:docs-video{src="../../assets/single-server/staging-create-form.webm" title="Creating a staging copy in the top bar of the Northwind Store project: the Subdomain, the address it builds and Create Staging"}

The panel shows `Staging Installation In Progress` and notifies you when the copy is ready. The copy appears under its live project, not as a separate project: as a `staging` tag on the live project's card, and as a `staging` tab in the project's top bar. Select either to open the copy.

The copy has no WordPress screens such as **Plugins** or **Users**. To change its files, use **Advanced > File Manager**, or **WP Admin** from the copy's **Actions** menu.

## Pushing staging to live

Push To Live copies the staging copy to the live project.

**Push To Live replaces what you tick on the live project with the staging copy's.** Check the staging copy first.

1. Open the copy and select **Push To Live** in its top bar.
2. Tick what to overwrite (see the options below).
3. Confirm. The panel shows `Push To Live In Progress` and notifies you when the push finishes.

:docs-video{src="../../assets/single-server/staging-push-to-live-dialog.webm" title="Push To Live on the staging copy: Overwrite files, Push full database and the table lists"}

What you can tick depends on the project.

- **A PHP hosting project** (a WordPress site or another PHP site) offers **Overwrite files**, **Push full database**, **Push structure changes in tables** and **Push tables with changed data**.
- **A container project** (a site from Git or from files) offers **Overwrite files** and **Overwrite volumes (including databases)**. It has no table lists.

### Pushing only some tables

On a PHP hosting project, you can push only some database tables. **Push structure changes in tables** lists the tables whose columns changed, and tables that exist only on the copy. **Push tables with changed data** lists the tables whose rows changed. Tick **Push full database**, then pick tables in either list to replace only those tables. A list is empty when nothing differs.

If the panel cannot compare the two databases, the window says `Could not compare database changes. You can still push files and database, but selective table changes are unavailable.` A push then copies the whole database.

## Pushing live to staging

Push To Staging copies the live project to its staging copy, so you can try changes on fresh content.

**Push To Staging replaces what you tick on the staging copy with the live project's.** Changes you made only on the copy are lost.

1. Open the live project and select **Push To Staging** in its top bar. The button shows only when the project has a staging copy.
2. Tick what to overwrite. A container project (from Git or from files) offers **Overwrite files** and **Overwrite volumes (including databases)**. A PHP hosting project offers **Overwrite files** and **Push full database**, with the table lists described in [Pushing staging to live](#pushing-staging-to-live).
3. Confirm. The panel shows `Push To Staging In Progress`.

::docs-video{src="../../assets/single-server/staging-and-preview-push-to-staging.webm" title="On the live Immich project with a staging tab, Push To Staging in the top bar opens the Push To Staging window with Overwrite files and Overwrite volumes (including databases), and Push To Staging shows Push To Staging In Progress"}
::

## Deploying a Git project to staging

You can deploy a branch of a Git project to its staging copy and leave production unchanged. The project needs a staging copy first: [Creating a staging copy](#creating-a-staging-copy).

1. Open the live project.
2. Select **Redeploy** on its **Source** card.
3. Tick **Deploy to staging environment**.

The chosen branch goes to the copy: [Git](git.md).

## Deleting a staging copy

Deleting a staging copy removes its files and databases. The live site stays as it is.

1. Open the copy and select **Delete Staging** in its top bar.
2. In the dialog, tick all three acknowledgements about files, databases and the removal.
3. Select **Delete Staging**. The button stays disabled until every box is ticked.

The panel shows `The staging site has been deleted.` The live project cannot be deleted while a staging copy exists: [Projects](projects.md).

## Preview link

A preview link opens the site on a temporary address on this server, so you can look at it before the real domain points here.

**The link works for 15 minutes.** After that, create a new one. **Anyone who has the link can open the site**, because a preview link has no password.

1. Open the project and select **Preview** in the top bar.
2. In **Preview Site**, select **Get Temporary URL**.
3. Select **Copy URL** to share it, or **Visit Site** to open it.

The domain warning on a project also has **Not live yet? Preview the site on the server** with a **Generate URL** button. On a project with a domain of your own, **Open Project** opens the same **Preview Site** window: [Projects](projects.md).

:docs-video{src="../../assets/single-server/staging-preview-link-window.webm" title="Open Project on a domain that does not point here yet opens Preview Site with Get Temporary URL, Copy URL and Visit Site"}

Something not working? Messages and fixes: [Troubleshooting: Projects](../troubleshooting/projects.md#staging-and-preview).
