---
title: "Removing the panel"
description: "Removing the panel deletes its project from the Engine. Your sites are separate projects and stay."
nav:
  group: Getting Started
  order: 6
---

# Removing the panel

Removing the panel deletes its project from the Engine. Your sites are separate projects and stay.

## Where the panel keeps its data

The panel keeps everything in one place inside its own project:

- your accounts and settings
- the project labels and the activity log
- the encrypted Engine access token and the panel's keys

The panel cannot back itself up. The backups you set up in **Settings** cover your sites, not the panel. Back the panel up with the Engine, on the server. A backup of the panel project includes its data volume, and protection does not stop it. If you have no backup store yet, create one first. `pae backup:container:create --help` lists the options.

```bash
pae project:backup:create panel --container=<store>
pae project:backup:list panel
```

To roll the panel back to a backup on the same Engine, run `pae project:backup:restore panel <id>`. **A restore overwrites the panel's current data.**

## Removing the panel

**This deletes the panel project and all its data. There is no undo.** Your other projects are not touched.

1. Run this on the server as `root`. `panel` is the project name you gave at install:

   ```bash
   pae project:delete panel
   ```

2. Confirm when `pae` asks.

`pae` works on a protected project. The panel cannot delete itself, but the command on the server can.

To remove the Engine and every project on it, follow the Engine's own removal steps. **That deletes every site.**

## Getting the panel back

To get a panel back, install again: [Installing the panel](install.md). A new install starts empty.

**Deleting the project also deletes its backup records on the Engine**, so you cannot restore into a new install with `pae project:backup:restore`. Before you delete, you can copy the items out of a backup:

```bash
pae project:backup:download panel <id> <item> --output=/opt/panelalpha/<file>
```

`pae project:backup:show panel <id>` lists the item numbers. The Engine has no command that reads such a copy into a new install, so a new panel starts with no accounts and no settings.
