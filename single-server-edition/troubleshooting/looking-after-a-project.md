---
title: "Looking after a project"
description: "Problems with backups, databases, files, FTP, SFTP, cron jobs and PHP settings, with what each message means and what to do."
nav:
  group: Troubleshooting
  order: 5
---

# Looking after a project

Problems with backups, databases, files, FTP, SFTP, cron jobs and PHP settings, with what each message means and what to do.

## Backups

**"Choose files or databases to back up."** Both parts are switched off. Switch at least one on to enable **Create**.

**"Select Restore Directory or Restore Database to continue."** or **"Select Restore data or Restore code to continue."** No part was chosen. Tick at least one to enable **Restore**.

**"Cannot delete a backup that is being created."** Wait until the backup has finished.

**"Backups are not available until the Engine has a backup container."** No backup store is working yet. Set one up in **Settings > Backups**.

**Automatic Backups shows `Unavailable` and "No backup store is set up."** This happens on a server that never got a backup store. Select **Set up store**, or open **Settings > Backups**, and choose one: [Backups](../looking-after-a-project/backups.md#setting-up-a-store).

**"This backup has no files to download."** The backup holds no file at all. Download another backup.

More about this: [Backups](../looking-after-a-project/backups.md).

## Databases

**"The project kind is not known yet, so its database cannot be renamed."** The panel has not yet learned what the project runs. Open the project page, wait a moment and try again.

**"The Engine could not rename the database."** The rename failed on the server. Check the tasks list and try again: [Tasks](../the-panel/tasks.md).

More about this: [Databases](../looking-after-a-project/databases.md).

## Files and access

**"The username field must not be greater than 32 characters."** or **"The user field must not be greater than 32 characters."** The SFTP or FTP account name is longer than 32 characters. Use a shorter name.

**"The username is too long for this project."** On **SFTP Accounts**, the panel puts a prefix before the name, and the prefix counts too. Use a shorter name.

**"Username can only contain letters and numbers."** On **FTP Accounts**, use only the letters a to z, A to Z and the digits 0 to 9. Underscores, hyphens, dots and spaces are refused.

**"Invalid file name."** The name has characters the file manager does not accept. Use a plainer name.

More about this: [Files and access](../looking-after-a-project/files-and-access.md).

## Usage and limits

**"Enter a value."** A limit field is empty or holds text that is not a number. Type a number, or tick **No limit** where the field has it.

**"The value must be at least 256 MB."** (or `6 MB`, `0.01 cores`, `1 MB`) The value is below the lowest the field takes. A container project needs at least 256 MB of memory, a PHP hosting project 6 MB. Raise the value.

**"The value must be at most ... ."** The value is above the highest the field takes. Lower it.

**A message from the server appears in the limits window.** The server can refuse a value the panel accepts, for example memory above what the server has left. Lower the value and save again.

**The limit is saved but the usage cards do not follow it.** Some limits apply only after the next rebuild: [Usage and limits of a project](../looking-after-a-project/statistics-and-logs.md#usage-and-limits-of-a-project).

## Cron jobs and PHP

**A cron job seems not to run.** The panel does not show what a cron job (a command that runs on a schedule) printed, so make it write a file you can check:

1. Make the command write a file in the site's folder, for example `date >> /home/<name>/example.com/cron-test.log`.
2. Wait for the schedule.
3. Open that file in **Advanced > File Manager**, in the folder of the domain.

If the file is missing, check that the command uses the whole path into the site's folder (the home folder is not writable), that the schedule is right, and, on a container project, that the chosen **Service** is running.

**"Invalid PHP directive: <name>."** The setting does not exist. Check the spelling.

**"Invalid directive value."** The value has a line break. Write it on one line.

**"The PHP version applies only to PHP hosting projects. A dind project runs PHP from its own image; set it there."** or **"Custom PHP INI settings apply only to PHP hosting projects. A dind project runs PHP from its own image; set php.ini there."** A dind project is a container project, and it brings its own PHP. Change the PHP version or `php.ini` in the project's own setup.

More about this: [Cron jobs and PHP](../looking-after-a-project/cron-and-php.md).
