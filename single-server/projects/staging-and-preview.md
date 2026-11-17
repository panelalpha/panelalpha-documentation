---
title: "Staging and preview"
description: "A staging copy is a second copy of a project where you can try changes before they reach the live site. A preview link shows a site on a temporary address..."
nav:
  group: Projects
  order: 6
---

# Staging and preview

A staging copy is a second copy of a project where you can try changes before they reach the live site. A preview link shows a site on a temporary address before its domain points to this server.

## Creating a staging copy

A project can have one staging copy, and a staging copy cannot have its own.

1. Open the project and select **Staging** in the top bar.
2. Enter a **Subdomain** for the copy. The panel adds the project's own domain after it, so on `example.com` the name `staging` gives `staging.example.com`; enter only that name, without dots. A site on the panel's free `panelalpha.online` address does not ask for a name: the copy gets its own free address there when it is created, and you cannot enter an address in that zone. For a WordPress site you can tick **Skip Uploads Directory** to leave out `wp-content/uploads`, which saves space and time.
3. Select **Create Staging**.

<!-- TODO screenshot: assets/staging-create-form.png. Show the Staging form with the Subdomain field (`staging`), the line under it with the address it builds (`staging.example.com`), the Skip Uploads Directory box and the Create Staging button. -->

The panel shows `Staging Installation In Progress` and notifies you when the copy is ready. The copy appears under its live project, not as a separate project: in **My Projects** its `staging` tag sits on the live project's card (and in the table view it is a row under the live project), and the project's top bar has a `staging` tab for it. Select that tab to open the copy; its address is in its own top bar once the copy is ready.

The project page of a staging copy has no WordPress screens such as **Plugins** or **Users**. To change the copy's files, use **Advanced > File Manager**, or **WP Admin** from the copy's **Actions** menu. A copy under `panelalpha.direct` has a self-signed certificate, so your browser warns when you open it.

## Pushing staging to live

**Push To Live replaces what you tick on the live project with the staging copy's.** Check the staging copy first.

1. Open the live project in **My Projects**, select its `staging` tag (or the `staging` tab in the top bar) and choose **Push To Live** in the copy's top bar. The same action is in the copy's **Actions** menu.
2. Tick what to overwrite. A PHP hosting project (a WordPress site or another PHP site) offers:
   - **Overwrite files**.
   - **Push full database**.
   - **Push structure changes in tables** and **Push tables with changed data**: the two lists hold the tables that differ between the copy and the live project. Tick **Push full database** and pick tables in either list to replace only those tables. Both lists are empty when nothing differs, and stay empty on a project without a database.
3. Confirm. The panel shows `Push To Live In Progress`.

<!-- TODO screenshot: assets/staging-push-to-live-dialog.png. Show the Push To Live dialog of a PHP hosting staging copy with Overwrite files, Push full database and the two table lists. -->

When the panel cannot compare the two databases, the window says `Could not compare database changes. You can still push files and database, but selective table changes are unavailable.` and the two lists are not shown: a push then copies the whole database.

A container project (a site from Git or from files) shows **Overwrite files** and **Overwrite volumes (including databases)** instead, without the two lists: the panel compares database tables only for PHP hosting projects.

A notification tells you when the push finishes.

## Deploying a Git project to staging

The **Redeploy** window offers **Deploy to staging environment** only once the project has a staging copy: a Git project without one shows no tick, so create the copy first ([Creating a staging copy](#creating-a-staging-copy)). Open the live project, select **Redeploy** on its **Source** card and tick it; the copy's own **Redeploy** window has no tick. The chosen branch is deployed to the copy and production is unchanged: [Git](git.md).

## Deleting a staging copy

Open the copy (its `staging` tag on the live project's card, or its `staging` tab) and select **Delete Staging** in its top bar. The panel asks you to confirm in a **Delete Staging** dialog: it names the copy and its domain and says the live site stays as it is. Tick all three acknowledgements:

- **I understand that the project's files will be deleted.**
- **I understand that the project's databases will be deleted.**
- **Yes, I understand and want to proceed with the removal of the staging site.**

The dialog's **Delete Staging** button stays disabled until every box is ticked. After it, the panel shows `The staging site has been deleted.` The live project cannot be deleted while a staging copy exists: [Projects](projects.md).

## Preview link

A preview link opens the site on a temporary address on this server, so you can look at it before the real domain points here. **The link works for 15 minutes.** After that, create a new one. Anyone who has the link can open the site, because a preview link has no password.

1. Open the project and select **Preview** in the top bar. You can also select **Open Project** on a project with a domain of your own: it opens **Preview Site**, whether or not that domain points to this server yet. On the panel's free `panelalpha.online` address **Open Project** opens the site itself in a new tab instead, so there is nothing to preview. In **My Projects** the button is on the card of a project that is not WordPress; a WordPress card has no **Open Project**, so open the project and select **Open Project** in its top bar (every project has it there).
2. In **Preview Site**, select **Get Temporary URL**.
3. Select **Copy URL** to share it, or **Visit Site** to open it.

The window shows `Your Site Temporary URL:`. While the first link is still valid, the panel shows it again.

<!-- TODO screenshot: assets/staging-preview-link-window.png. Show the Preview Site window with "Your Site Temporary URL:", an example address, and the Copy URL and Visit Site buttons. -->

The domain warning on a project also has **Not live yet? Preview the site on the server** with a **Generate URL** button.

## Troubleshooting

**"This project already has staging environment."** The project has a staging copy. Use it, or delete it and create a new one.

**"Cannot create staging project from staging project."** You cannot copy a staging copy. Create the copy from the live project.

**"A staging copy cannot use a panelalpha.online address: the PanelAlpha proxy assigns those names. Create the copy without a domain to get a free one."** The panel assigns the addresses in the free `panelalpha.online` zone itself, so the copy cannot be given one. Create the copy without a domain, or with an address on a domain of your own.

**"Subdomain must be a single name, without dots"** The panel adds the project's domain after the name, so enter only the name: `staging`, not `staging.example.com` (that would give `staging.example.com.example.com`).

**"The project is still being prepared. Please try again in a moment."** The project is not ready yet. Wait and try again.

**"The project has no live parent to push to."** The project is not a staging copy. **Push To Live** works only from a staging copy.

**"Could not compare database changes. You can still push files and database, but selective table changes are unavailable."** The panel could not read the tables. Push the full database or only files.

**"A preview tunnel is already being created for this project. Try again in a moment."** Another preview is being made. Wait a moment and select the button again.
