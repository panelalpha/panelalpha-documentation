---
title: Backups
description: "A backup is a complete snapshot of one project. For a traditional PHP project that is its home folder and one dump of each of its databases."
nav:
  group: Capabilities
  order: 3
---

# Backups

A backup is a complete snapshot of one project. For a traditional PHP project it is the home folder and one dump of each database. For a container project it is the files, the stored data volumes and the databases.

Know two things before you rely on backups:

1. **Backups run when you ask, or on a schedule you set.** Without a schedule, nothing is backed up by itself: [Scheduled backups](#scheduled-backups).
2. **A traditional PHP backup does not include the project's PHP settings** (version and limits). Those belong to each domain: [PHP version and settings](domains-and-ssl.md#php-version-on-traditional-php-hosting).

## First, somewhere to put them

A backup has to be written somewhere. That place is called a **store**. You set one up once, and every later backup goes into it.

**The engine sets one up for you.** An install creates a local store named `local` in `/opt/panelalpha/backups/local` and marks it the **default**. The default is the store a panel uses when nobody has chosen one. A fresh install can therefore back up a project at once.

An Engine update does the same on a server that has no store at all. On a server that has a store, it changes nothing: your stores stay as they are, and the oldest is the default.

You can change the default. `GET /system/info` reports it as `default_backup_container`, and `GET /backup-containers` marks it `"default": true`. To make another store the default, ask the assistant or use `pae backup:container:update <store> --default`.

Deleting the default store works like deleting any other. The oldest store left becomes the default. On a server with no store, `pae backup:container:ensure-default` (which the installer runs) creates `local` again.

| Kind | Where the files go |
|---|---|
| `local` | A folder on this VPS |
| `s3` | An S3-compatible bucket |
| `ftp` | A folder on an FTP server |
| `ftps` | Same, over TLS |
| `sftp` | A folder on an SSH server |

If the assistant cannot create a store, see [Decide what the assistant may do](../connecting-your-ai/your-assistant.md#decide-what-the-assistant-may-do).

A `local` store writes from inside the engine container. Its folder must be under one of the three paths the engine keeps on the host: `/home`, `/opt/panelalpha` or the engine's own storage volume (`/var/www/html/storage`).

The engine refuses a store at any other path, such as a plain `/var/backups`, with a 422 when you create or change the store. A folder there would sit in the container's writable layer and be lost the next time the container is recreated, while the backup rows still point at it.

The engine also refuses these places:

- `/opt/panelalpha/shared-hosting` and anything below it. Only the engine's own files may be written there, so every backup into it would fail.
- `/home`, `/opt/panelalpha` and `/var/www/html/storage` themselves, because the engine depends on them. Use a folder below them.

**A store that already sits at one of these paths refuses every change, a rename included, until you move it** to a folder such as `/opt/panelalpha/backups/`.

Keep local stores under `/opt/panelalpha/backups/`. Avoid a folder directly in `/home`. Every project's home is `/home/<project name>`, so `/home/backups` would be the home of a project named `backups`.

```text
Create a backup store on this host at /opt/panelalpha/backups.
```

**A store on the same VPS as the engine does not protect you from losing the VPS.** It is fine when you are about to do something risky and want to be able to undo it. To protect against disk failure or a lost VPS, use a remote store.

Check that the engine can reach a remote store before you depend on it:

```text
Test that backup store.
```

When the test fails, the answer says what failed in one line: the kind of store, the host, the folder or bucket, and the reason the storage gave. Passwords and keys in it are hidden.

## Taking a backup

```text
Create a backup of this project.
```

You get a backup ID, a status and the ID of the task that takes the backup (`backup_task_id`; a restore and a delete have `restore_task_id` and `delete_task_id`). Wait for `completed`. A backup of a large site takes a while.

A backup that names no store goes to the default one. On a server with no default store it is refused with `no_default_container`. Create a store, or name one.

A backup, a restore, a push and a staging copy never run on the same project at the same time. The second one is refused as busy, and a scheduled backup tries again.

A backup or restore stops the app while it works. It always starts the app again, also when the backup or restore fails. One time limit covers all the steps, so it cannot hang with the app stopped.

You can back up only the files or only the databases, and add a note to find the backup later. On a container project, "databases" also covers the stored data volumes.

```text
Back up this project, databases only, with the note "before the plugin update".
```

## Restoring

```text
List the backups for this project.
Restore last backup onto this project.
```

**A restore overwrites the live project.** Anything created since that backup was taken is gone. List the backups and check the date of the one you want before you confirm. Your assistant asks you to confirm, and that confirmation is required. A backup that is not `completed` cannot be restored, and neither can a protected project: [Protecting](projects.md#protecting).

On a traditional PHP project:

- A restore replaces the home folder with the backed-up one. It puts the old one back if the restore fails.
- To keep the files that are there and only write the backed-up ones over them, say so.
- You can restore only the databases or only the files, if the backup has them.
- A restore that fails puts the databases back as they were.
- A restore of a classic WordPress site makes its `wp-config.php` private again.

Choosing parts by volume is for container projects only.

You choose the parts with `only` or `exclude`, never both. Each is an object with up to three keys: `files` (true or false), `volumes` (a list of volume names) and `databases` (a list of database names). For example, `only` with `databases` set to one database name restores only that database.

## Deleting

**Deleting a backup removes its files, and that copy of the project can never be restored.** Deleting a store that still holds backups is refused unless you ask to delete the backups with it. Then they are gone for good. The assistant says what will be lost and asks you to confirm first.

## Download one part

A backup is stored as separate items, for example the home folder and one dump per database. To take one item off the server, ask:

```text
Download the database dump from the newest backup of this project.
```

The assistant reads only a small piece of a file at a time. For a large item, use the server command: [CLI commands](../commands/pae-cli.md#backups).

## Scheduled backups

Each project can have one automatic schedule. You give it a store, a time and how many backups to keep. A new schedule without a store uses the default one. A changed schedule keeps the store it has.

```text
Back up this project every night at 03:00 into the store local-main, and keep
the last 14.
```

Times are in UTC, not the server's local time. A schedule can run at most once an hour. The number to keep is from 1 to 365.

- When there are more backups than that number, the schedule deletes only the backups it made. Backups you took yourself are never deleted by it.
- If the project is busy at the planned time, the engine tries again for up to an hour, then skips that run.
- On the server, `backups:run-scheduled` prints what it did for each schedule that was due.
- A store that a project's enabled schedule uses cannot be deleted. The refusal names those projects. Turning the schedule off releases the store: a disabled schedule holds nothing, and deleting the store removes it.

To look at the schedule or turn it off, ask:

```text
Show the backup schedule of this project. Then remove it.
```

## Troubleshooting

**A 404 error when creating a backup.**
The store name or ID is wrong. Ask to list the stores first and use the exact name.

**The backup fails partway on a large site.**
The cause is usually disk space on the destination or a timeout to a remote store. Check the free space on the target, and test the store.

**How do I schedule these?**
Ask the assistant to set a schedule: [Scheduled backups](#scheduled-backups). Do not use a project's own cron jobs for this. They run inside the project and cannot reach the engine's backup system.

**"Project is busy: a backup is running" or "a restore is running".**
A backup, a restore and a deploy do not run on the same project at the same time. Wait for the one that is running, then try again.

**"Choose files or databases to back up."**
Both were switched off. Leave at least one on.

## More requests to try

**Mode** is the lowest permission mode that allows the request: read, modify or full ([Decide what the assistant may do](../connecting-your-ai/your-assistant.md#decide-what-the-assistant-may-do)). The examples assume a repository project named `tshop`. A **store** is what the tools call a backup container.

**A local store**

```text
Create a backup store called local-test on this host at /opt/panelalpha/backups/test.
Test that it works, then list the stores.
```

Mode: modify. Tools: `backup_container_create` with `driver` local, `backup_container_test`, `backup_container_list`, `backup_container_get`. Check: the test passes, and the store is in the list.

**A remote store**

```text
Create a backup store called s3-test on an S3 bucket, and test it. Do not ask
me to type the access keys into this chat.
```

Mode: modify. Tools: `backup_container_create` with `driver` s3 and `credentials`, `backup_container_test`. Check: the assistant does not ask for the keys in the chat, and the test passes. `ftp`, `ftps` and `sftp` are the other drivers. Test each one the same way before you rely on it.

**Change or remove a store**

```text
Rename the store local-test to local-main, point it at
/opt/panelalpha/backups/main, then delete the store s3-test.
```

Mode: full. Tools: `backup_container_update`, `backup_container_delete`. Check: the changes read back from `backup_container_get`, and the deleted store is gone.

**Make a store the default**

```text
Make the store local-main the default one.
```

Mode: modify. Tools: `backup_container_update` with `default`. Check: `backup_container_list` marks `local-main` and no longer marks the store that was the default.

**Take a backup and follow it**

```text
Back up tshop into the store local-main. Tell me when it is completed.
```

Mode: modify. Tools: `backup_create` with `container`, `task_get`, `backup_get`. Check: an id and a status come back, and the assistant waits for `completed`. On a traditional PHP project the backup lists a home item and one item per database.

**A backup without naming a store**

```text
Back up tshop. Which store did it go to?
```

Mode: modify. Tools: `backup_create`, `backup_get`, `backup_container_list`. Check: the backup is in the store marked `default` in the list. On a server with no default store the request is refused as `no_default_container`.

**Only files or only databases**

```text
On the traditional PHP project tphpsite, back up only the databases into
local-main with the note "before the update". Then try a backup with neither
files nor databases.
```

Mode: modify. Tools: `backup_create` with `databases`, `files` and `note`, `backup_get`. Check: the backup has database items and no home item, the note shows in `backup_list` and the request with both off is refused as `nothing_to_back_up`.

**List and inspect**

```text
List the backups of tshop with their dates and sizes, and show the newest one.
```

Mode: read. Tools: `backup_list`, `backup_get`. Check: the dates are right, so the assistant can pick the correct one before a restore.

**Restore all, then only part**

```text
Restore the newest backup onto tshop. Then restore only the database from an
older one, leaving the files as they are.
```

Mode: modify. Tools: `backup_restore` with `confirm`, then with `only` or `exclude` and `delete_existing_files`. Check: the assistant says a restore overwrites the live project and asks first. After the partial restore the files are untouched and the site still serves the application (`app_health_check`). On a traditional PHP project, a volume filter is refused as `volumes_not_applicable`.

**Download one item**

```text
List the items of the newest backup of tshop and download the database dump.
```

Mode: read. Tools: `backup_get`, `backup_item_download` with `item`, `offset` and `length`. Check: the item comes back in pieces of at most 49152 bytes with a `next_offset`. A backup that is not completed answers `backup_not_completed`.

**A backup schedule**

```text
Back up tshop every night at 03:00 into local-main and keep the last 7. Show
the schedule, then try every 10 minutes, then remove it.
```

Mode: modify to set it, full to remove it. Tools: `backup_schedule_set` with `container`, `cron` and `keep_last`, `backup_schedule_get`, `backup_schedule_delete`, `backup_list` with `kind` scheduled. Check: the schedule reads back in UTC, the 10-minute one is refused as `cron_too_frequent`, and scheduled backups are listed with `kind` scheduled.

**A store in a refused place**

```text
Create a backup store called bad-store on this host at /opt/panelalpha/shared-hosting/backups.
```

Mode: modify. Tools: `backup_container_create`. Check: the store is refused with the reason that only the engine's own files may be written there, and nothing is created.

**Restore only some parts**

```text
Restore only the databases from the newest backup of tshop. Then restore
everything except the files.
```

Mode: modify. Tools: `backup_restore` with `only` and with `exclude`. Check: the first restores only the databases and the second restores everything but the files.

**A backup while a push runs**

```text
Back up tshop while a push to its staging copy is running.
```

Mode: modify. Tools: `backup_create`, `project_push`, `task_get`. Check: the backup is refused as busy, and works once the push has finished. After any backup the app is running again. Skip when you cannot start both within the push's run time.

**Delete a backup**

```text
Delete the oldest backup of tshop.
```

Mode: full. Tools: `backup_delete`, `backup_list`. Check: only that backup is gone, and a folder the delete leaves empty is removed too. Deleting a local store that holds nothing removes its folder.

## From the server

Creating and restoring backups, setting up a store, scheduling and downloading: [CLI commands](../commands/pae-cli.md#backups). The schedule and the download have their own commands, `pae project:backup:schedule` and `pae project:backup:download`.
