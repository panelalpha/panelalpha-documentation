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

A backup of a container project holds two parts: **Code** and **Data**. Code is the project's files. Data is what the app saved while running, including its databases. For Immich, the photos and the database are Data. The files that came from the Git repository are Code.

The **Backups** tab lists every backup of the project, newest first. Each row has these columns:

- **Date:** when the backup was made.
- **Data** and **Code:** a green check means that part is inside the backup. A backup made with only one part ticked shows a check for that part only.
- **Size:** how much space the backup takes. A backup that is still running shows **Creating Backup** instead.
- **Type:** **Manual** for a backup you made, **Automatic** for one made by the schedule.
- **Note:** the text you wrote when you made the backup.

At the end of each row are three buttons: download, restore and delete. Tick the box in the first column to select a row. The box in the header selects all rows. Type in **Enter search term** to find a backup by its note or date.

::docs-video{src="../../assets/single-server/backups-container-code-data-rows.webm" title="The Backups tab of Immich: the three backups with the columns Date, Data, Code, Size, Type and Note, and the pointer moving over each row and the Code column"}
::

The two kinds of project: [Projects](../projects/projects.md).

## Making a backup

1. Open the project and go to the **Backups** tab, **Backup History**.
2. Select **Add New**.
3. In **Create Backup**, write a **Backup Note** if you want one. The note helps you remember why you made the backup, for example `Before the update`. You can leave it empty. On a PHP hosting project, keep **Backup Directory** and **Backup Database** ticked for the parts you want. A container project has no ticks: its backup always holds the code and the data.
4. Select **Create**. Select **Cancel** to close the window without a backup.

   :docs-video{src="../../assets/single-server/backups-create-dialog.webm" title="Making a backup of Immich: Add New opens Create Backup, Create starts it, the new row shows Creating Backup and then its size 615 MB with the type Manual"}

**A container project is stopped while its code and data are copied.** It starts again right after.

A finished backup has a size and a **Type**: **Manual** or **Automatic**. A backup with only one part ticked downloads, restores and deletes like any other.

## Automatic backups

Each project can have a schedule. New projects get one by default: [Settings](../the-panel/settings.md). You can change or stop it.

1. On the **Backups** tab, switch **Automatic Backups** on. The **Automatic Backups** dialog opens.
2. Choose **How Often**: **Daily**, **Weekly** or **Monthly**.
3. Choose when. **Choose Time** sets the time of day. **Weekly** also asks for **Choose Day**, one day from Mon to Sun. **Monthly** asks for the day of the month.
4. Select **Enable Automatic Backups**. When a schedule already exists the button reads **Edit Automatic Backups**.

   :docs-video{src="../../assets/single-server/backups-automatic-dialog.webm" title="The schedule of Immich: Change beside Every day at 2:00 AM opens Automatic Backups, How Often switches from Daily to Weekly, which adds Choose Day, then back to Daily with Choose Time, and the button Edit Automatic Backups"}

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
