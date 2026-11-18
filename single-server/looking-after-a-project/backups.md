---
title: "Backups"
description: "A backup is a copy of one project that you can restore later. This page shows how to make, schedule, restore and download backups."
nav:
  group: Looking After a Project
  order: 3
---

# Backups

A backup is a copy of one project that you can restore later. Backups work out of the box. They are stored on the same VPS as the panel. **A copy on the same server is lost if the server is lost.** For real protection, set up a store outside the server: [Setting up a store](#setting-up-a-store).

Backups are written to a **store**, a place that is set up once for the whole panel. **Settings > Backups** shows the store in use. A store made on the server earlier is kept and used: [On the server](../getting-started/on-the-server.md#backup-stores-made-on-the-server).

## Setting up a store

Choose the store in **Settings > Backups**: [Settings](../the-panel/settings.md). You can use S3, SFTP, FTP or FTPS to keep backups away from the server. Select **Remove store** there to go back to the store the server came with.

A server installed long ago and never updated may have no store. The **Backups** tab of a project on it shows `No backup store is set up, so backups cannot be created.` and a button **Set up backup store**. The button opens **Settings > Backups**. **Create Backup** is greyed out until a store is set up.

## What a backup holds

A backup of a PHP hosting project holds its files (**Directory**) and its databases (**Database**).

A backup of a container project holds two parts: **Code** and **Data**. Code is the project's files. Data is what the app saved while running, including its databases. The **Backups** tab shows them as separate columns.

:docs-video{src="../../assets/single-server/backups-container-code-data-rows.webm" title="The Backups tab of the ntfy container project: each backup has its own Code and Data columns, and pointing at an entry shows what it holds"}

The two kinds of project: [Projects](../projects/projects.md).

## Making a backup

1. Open the project and go to the **Backups** tab, **Backup History**.
2. Select **Add New**.
3. In **Create Backup**, write a **Backup Note** if you want one. On a PHP hosting project, keep **Backup Directory** and **Backup Database** ticked for the parts you want.
4. Select **Create**.

   :docs-video{src="../../assets/single-server/backups-create-dialog.webm" title="Making a backup of the Northwind Journal WordPress project: Add New, the note Before installing WooCommerce, Backup Directory and Backup Database ticked, Create, then the row shows Creating Backup and finally its size and the type Manual"}

**A container project is stopped while its code and data are copied.** It starts again right after.

A finished backup has a size and a **Type**: **Manual** or **Automatic**. A backup with only one part ticked downloads, restores and deletes like any other.

## Automatic backups

Each project can have a schedule. New projects get one by default: [Settings](../the-panel/settings.md). You can change or stop it.

1. On the **Backups** tab, switch **Automatic Backups** on. The **Automatic Backups** dialog opens.
2. Choose **How Often**: Daily, Weekly or Monthly. Then choose the time and, for weekly or monthly, the day.
3. Select **Enable Automatic Backups**.

   :docs-video{src="../../assets/single-server/backups-automatic-dialog.webm" title="Changing the automatic backups of the Northwind Journal project: Change opens Automatic Backups with How Often set to Daily, a new time is picked in Choose Time and Edit Automatic Backups saves it"}

To change a schedule, select **Change** beside it. To stop it, switch **Automatic Backups** off and select **Disable**. The time is shown in your local time.

### The default schedule

New projects get a default schedule from **Settings > Backups**. A project that already exists keeps its own schedule.

## Restoring a backup

**A restore overwrites the project. Anything made since the backup is gone.**

1. Go to the **Backups** tab and select **Restore** on the backup.
2. Choose what to restore.
3. Confirm.

**On a PHP hosting project**, tick **Restore Database** and **Restore Directory**. **Delete Existing Files** is ticked by default. If you untick it, existing files stay and may use extra disk space.

**On a container project**, tick **Restore data (volumes and databases)** and **Restore code**. Both start unticked, so tick at least one. **The project is stopped while the backup is restored. Restoring code replaces all files in the project directory. Restoring data replaces the volumes and databases from this backup.**

## Downloading and deleting

**To download**, select **Download Backup**. The file opens in a new window. Allow pop-ups for the panel if it does not.

If the backup is split into several files, the panel asks which part you want in **Backup Type**. One download holds one part, so download the other parts the same way.

**To delete**, select the delete action and tick the confirmation. You can also select several rows and delete them together.

Something not working? Messages and fixes: [Troubleshooting: Looking after a project](../troubleshooting/looking-after-a-project.md#backups).
