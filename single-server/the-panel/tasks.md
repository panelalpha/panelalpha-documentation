---
title: "Tasks"
description: "Tasks lists the work PanelAlpha Engine does for your projects, such as deploys, backups and staging copies, with its status and how long it took."
nav:
  group: The Panel
  order: 2
---

# Tasks

Tasks lists the work PanelAlpha Engine, the hosting software the panel runs on, does for your projects, such as deploys, backups and staging copies. Each entry shows its status and how long it took.

Open it from the user menu with **Tasks**. The menu item shows a dot while a task runs.

## Reading the list

Tabs under the title filter the list by status: **All**, **Active**, **Failed**, **Completed** and **Cancelled**. **Active** shows the pending and running tasks together.

Tasks are grouped by day: `Today`, `Yesterday`, then the date. Each row has five columns.

- **Task**: what was done. A push from your Git host shows as `Git auto-deploy`. A failed task shows the first line of its error under the name.
- **Project**: a link to the project. Housekeeping tasks of the server have no project and show a dash.
- **Started**: the time the task began and how long ago. A task that has not begun shows `Waiting`.
- **Duration**: how long the task ran, or has been running so far.
- **Status**: `Pending` (waiting to start), `Running`, `Completed`, `Failed` or `Cancelled`.

On a phone each task is a compact card.

::docs-video{src="../../assets/single-server/tasks-list.webm" title="Tasks opens from the user menu with the status tabs All, Active, Failed, Completed and Cancelled, the Project and Task type lists, and the tasks grouped under Today and Yesterday with their Project, Started, Duration and Status"}
::


## Task details

Select a row to open its details: **Queued**, **Started**, **Finished** and **Task ID**. For a failed task the details also show the full **Error**, with a button to copy it.

## Finding a task

1. Open **Tasks**.
2. Select a status tab. Use **Active** to see the pending and running tasks together.
3. Set **Task type** to narrow the list to one kind of work.

If nothing matches, you see `No tasks` and `There are no tasks matching the filters.`

For a failed deploy, the deploy log of the project has the detail: [Git](../projects/git.md).
