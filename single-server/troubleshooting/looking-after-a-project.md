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

**"Choose files or databases to back up."** Both parts are switched off. The dialog shows the warning and **Create** stays off until you switch at least one part on.

**"Select Restore Directory or Restore Database to continue."** No part was chosen. The dialog shows the warning and **Restore** stays off until you tick at least one part.

**"Select Restore data or Restore code to continue."** The same on a container project, where both parts start unticked. Tick at least one.

**"Cannot delete a backup that is being created."** Wait until the backup has finished.

**"Backups are not available until the Engine has a backup container."** No store is working yet. Set one up in **Settings > Backups**.

**"This backup has no files to download."** The backup holds no file at all. Download another backup.

More about this: [Backups](../looking-after-a-project/backups.md).

## Databases

**"The project kind is not known yet, so its database cannot be renamed."** The panel has not yet learned what the project runs. Open the project page, wait a moment and try again.

**"The Engine could not rename the database."** The rename failed on the server. Check the tasks list and try again: [Tasks](../the-panel/tasks.md).

More about this: [Databases](../looking-after-a-project/databases.md).

## Files and access

**"The username field must not be greater than 32 characters."** On **SFTP Accounts**, the name you type may not be longer than 32 characters. Use a shorter name.

**"The username is too long for this project."** On **SFTP Accounts**, the panel puts a prefix before the name, and the prefix counts too. A name of 32 characters or fewer is still refused when the prefix makes it longer. Use a shorter name.

**"The user field must not be greater than 32 characters."** On **FTP Accounts**, the name may not be longer than 32 characters. Use a shorter name.

**"Username can only contain letters and numbers."** On **FTP Accounts**, a name with an underscore, hyphen, dot or space is refused. Use only the letters a to z, A to Z and the digits 0 to 9.

**"Invalid file name."** The name has characters the file manager does not accept. Use a plainer name.

More about this: [Files and access](../looking-after-a-project/files-and-access.md).

## Cron jobs and PHP

**A cron job seems not to run.** The panel does not show what the job printed, so make the job write a file you can check:

1. Make the command write a file in the site's folder, for example `date >> /home/<name>/example.com/cron-test.log`.
2. Wait for the schedule.
3. Open that file in **Advanced > File Manager**, in the folder of the domain.

If the file is missing, check these:

- The command uses the whole path into the site's folder. The home directory is not writable, and a relative path starts there.
- The schedule is right.
- On a container project, the chosen **Service** is running.

**"Invalid PHP directive: <name>."** The setting does not exist. Check the spelling.

**"Invalid directive value."** The value has a line break. Write it on one line.

**"The PHP version applies only to PHP hosting projects. A dind project runs PHP from its own image; set it there."** The project is a container project. Change PHP in the project's own image.

**"Custom PHP INI settings apply only to PHP hosting projects. A dind project runs PHP from its own image; set php.ini there."** Same cause. Set `php.ini` in the project's own image.

More about this: [Cron jobs and PHP](../looking-after-a-project/cron-and-php.md).
