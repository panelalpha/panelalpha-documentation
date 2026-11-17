---
title: "Cron jobs and PHP"
description: "A cron job runs a command on a schedule that you choose. PHP settings set the PHP version and limits of a PHP hosting project. Both are under Advanced."
nav:
  group: Looking After a Project
  order: 5
---

# Cron jobs and PHP

A **cron job** runs a command on a schedule that you choose. PHP settings set the PHP version and limits (such as memory) of a PHP hosting project. Both are in the project under **Advanced**.

**PHP version and PHP settings are not available for container projects.** They run PHP from their own image: [Projects](../projects/projects.md).

## Adding a cron job

1. Open the project, go to **Advanced > Cron Jobs** and select **Add Cron Job**.
2. In **Common Settings**, pick how often it runs, for example **Every 5 minutes (*/5 * * * *)** or **Once a day**. Choose **Custom** to write your own **Cron Expression**.
3. Enter the **Command**. It must be on one line.
4. On a container project, choose the **Service** the command runs in. **A run is skipped while that service is not running.**
5. Save. The panel shows `Cron job has been added.`

   ![Adding a cron job to the Northwind Journal project: Add Cron Job, Common Settings set to Once a day, the command wp cron event run --due-now, and the new job in the list](../../assets/single-server/cron-add-form.gif)

You can edit or delete a job from its row.

While you write a **Cron Expression**, the **Frequency** line of the form reads back what the expression means. It says `Invalid Expression` while it cannot be read. A schedule the server refuses is not saved. The panel shows the reason, for example `Value 99 out of bounds (0-59)` or `Range values out of bounds in '10-70' (allowed 0-59)`.

A container project with no containers yet shows `This project has no container services yet. Deploy or start the project, then add a cron job.`

### Where the command runs

On a PHP hosting project the command runs inside the project's container as the project's user. It starts in the project's home directory, `/home/<name>`. **The home directory itself is not writable**, so write files in the site's folder instead.

The **Document Root** in **Advanced > Domains** is relative to the home directory, so `/example.com/public_html` is `/home/<name>/example.com/public_html`. Write the job's output in the folder of the domain that holds the document root. That folder belongs to the project's user and is **not served**.

```
date >> /home/<name>/example.com/cron.log
```

A relative path starts in the home directory, so give the whole path.

**Never write the job's output in the document root, `public_html`.** Every visitor of the site can read files there.

## Changing the PHP version

**The PHP version applies to a domain, so it changes all projects on that domain.**

1. Open the project and go to **Advanced > Developer Settings**.
2. In the card **PHP Version**, select **Manage**.
3. In **Set PHP Version**, choose the version for the domain.
4. Save.

The card shows `Current PHP version: <version>`. The Engine decides which versions you can choose.

## Changing PHP settings

1. Go to **Advanced > Developer Settings**.
2. In the card **PHP Settings**, select **Manage**.
3. Set the value of a setting. The dialog always lists `memory_limit`, `upload_max_filesize`, `post_max_size` and `max_execution_time`, with the value this project sets for it. **Add Directive** takes any other setting.
4. Save.

A saved setting reaches the site within a few seconds. The dialog shows the saved value as soon as you save it.

An empty value means this project does not set that setting, so the server's PHP configuration applies. The dialog does not show the values the server uses. To use a setting the project does not set yet, type the value you want. To go back to the server's value, clear the value and save. The panel removes the setting and the server's PHP configuration applies again.

### More detail

PHP keeps the settings it reads for a folder in a cache for a few seconds. A request the server handles before that cache is refreshed still uses the old value.

![Advanced > Developer Settings of the Northwind Journal project: the PHP Version card with Current PHP version, then Manage on PHP Settings, memory_limit set to 256M and Save Changes](../../assets/single-server/php-settings-dialog.gif)

## Troubleshooting

**A cron job seems not to run.** The panel does not show what the job printed, so make the job leave a trace:

1. Make the command write a file next to the document root, for example `date >> /home/<name>/example.com/cron-test.log`.
2. Wait for the schedule.
3. Open that file in **Advanced > File Manager**, in the folder of the domain.

If the file is missing, check these:

- The command writes the whole path into the site's folder. The home directory is not writable and a relative path starts there.
- The schedule is right.
- On a container project, the chosen **Service** is running.

**"Invalid PHP directive: <name>."** The setting does not exist. Check the spelling.

**"Invalid directive value."** The value has a line break. Write it on one line.

**"The PHP version applies only to PHP hosting projects. A dind project runs PHP from its own image; set it there."** The project is a container project. Change PHP in the project's own image.

**"Custom PHP INI settings apply only to PHP hosting projects. A dind project runs PHP from its own image; set php.ini there."** Same cause. Set `php.ini` in the project's own image.
