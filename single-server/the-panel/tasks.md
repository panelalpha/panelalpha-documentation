---
title: "Tasks"
description: "Tasks lists the work the Engine does for your projects, such as deploys, backups and staging copies, with its status and how long it took."
nav:
  group: The Panel
  order: 2
---

# Tasks

Tasks lists the work the Engine does for your projects, such as deploys, backups and staging copies, with its status and how long it took.

Open it from the user menu with **Tasks**. The menu item shows a dot while a task runs.

## Reading the list

Each row has five columns.

- **Task**: what was done. A push from your Git host shows as `Git auto-deploy`. Other names are listed under [Task names](#task-names).
- **Project**: a link to the project, with its name and domain. A task for a project the panel does not know shows a dash. Hover over it to see `Not a panel project`.
- **Started**: how long ago the task began, such as `now`. A task that has not begun shows `Waiting`. Hover to see the date and time in your date format.
- **Duration**: for example `45s`, `3m 20s` or `1h 5m`. A running task counts up to now. A task that has not begun shows a dash.
- **Status**: `Pending`, `Running`, `Completed`, `Failed` or `Cancelled`.

Hover over a failed task to see why it failed, when the Engine gave a reason.

![Tasks opened from the user menu: deploys, backups and variable changes with their project, start time, duration and status, and the Status filter](../../assets/single-server/tasks-list.gif)

## Finding a task

1. Open **Tasks**.
2. Set **Status** to a single state, or to `Active` to see the pending and running tasks together.
3. Set **Task type** to narrow the list to one kind of work.

The list shows 50 tasks per page. You can change that to 15 or 100. It refreshes every 5 seconds while a task is pending or running, and every 30 seconds otherwise.

With no match you see `No tasks` and `There are no tasks matching the filters.`

For a failed deploy, the deploy log of the project has the detail: [Git](../projects/git.md).

## Task names

The **Task** column uses these names: Deploy, Rebuild, Deploy archive, Attach source, Git pull, Change branch, Revert, Create backup, Restore backup, Delete backup, Delete backup container, Create staging, Push staging, Apply environment variables, Apply API access, Rename database, Copy project, Publish base image, Refresh base image, Remove base image and Build repository cache.

The last four are the server's own housekeeping. A base image is an image a deploy builds from. The host build cache is what makes a repeat deploy of the same repository faster. These tasks belong to no project, so their **Project** is a dash.
