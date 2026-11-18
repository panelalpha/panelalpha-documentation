---
title: "Cron jobs and PHP"
description: "A cron job runs a command on a schedule that you choose. PHP settings set the PHP version and limits of a PHP hosting project. Both are under Advanced."
nav:
  group: Looking After a Project
  order: 5
---

# Cron jobs and PHP

A **cron job** is a command that the server runs again and again on a schedule, for example every night. PHP settings set the PHP version and limits (such as memory) of a PHP hosting project. Both are in the project under **Advanced**.

**PHP version and PHP settings are not available for container projects.** They run their own PHP: [Projects](../projects/projects.md).

## Adding a cron job

1. Open the project, go to **Advanced > Cron Jobs** and select **Add Cron Job**.
2. In **Common Settings**, pick how often it runs, for example **Every 5 minutes (*/5 * * * *)** or **Once a day**. Choose **Custom** to write your own **Cron Expression**. The **Frequency** line reads it back in words.
3. Enter the **Command**. It must be on one line.
4. On a container project, choose the **Service** the command runs in. **A run is skipped while that service is not running.**
5. Save.

   :docs-video{src="../../assets/single-server/cron-add-form.webm" title="Adding a cron job to the Northwind Journal project: Add Cron Job, Common Settings set to Once a day, the command wp cron event run --due-now, and the new job in the list"}

You can edit or delete a job from its row. If the schedule is not valid, the panel does not save it and shows the reason.

### Where the command runs

On a PHP hosting project the command runs from the project's home folder, `/home/<name>`. **You cannot write files in the home folder itself**, so write them in the site's folder. For example, this command writes a line to a log file in the folder of `example.com`:

```
date >> /home/<name>/example.com/cron.log
```

**Never write the job's output in `public_html`.** Every visitor of the site can read files there.

## Changing the PHP version

**The PHP version applies to a domain, so it changes all projects on that domain.**

1. Open the project and go to **Advanced > Developer Settings**.
2. In the card **PHP Version**, select **Manage**.
3. In **Set PHP Version**, choose the version for the domain.
4. Save.

## Changing PHP settings

1. Go to **Advanced > Developer Settings**.
2. In the card **PHP Settings**, select **Manage**.
3. Set the value of a setting. **Add Directive** takes any other setting.
4. Save. The site uses the new value within a few seconds.

The dialog always lists four settings:

- `memory_limit`: how much memory one PHP request may use.
- `upload_max_filesize`: the largest single file a visitor can upload.
- `post_max_size`: the largest amount of data one form submission can send, uploads included. Keep it at least as large as `upload_max_filesize`.
- `max_execution_time`: how many seconds a PHP request may run before it is stopped.

An empty value means this project does not set that setting, so the server's own value applies. To go back to it, clear the value and save.

:docs-video{src="../../assets/single-server/php-settings-dialog.webm" title="Advanced > Developer Settings of the Northwind Journal project: the PHP Version card with Current PHP version, then Manage on PHP Settings, memory_limit set to 256M and Save Changes"}

Something not working? Messages and fixes: [Troubleshooting: Looking after a project](../troubleshooting/looking-after-a-project.md#cron-jobs-and-php).
