---
title: "Removing the panel"
description: "Removing the panel deletes its project from PanelAlpha Engine. Your sites are separate projects and stay."
nav:
  group: Getting Started
  order: 7
---

# Removing the panel

Removing the panel deletes its project from PanelAlpha Engine, the hosting software it runs on. Your sites are separate projects and stay.

## Backing up the panel

The panel keeps your account, settings and activity log in its own project. The backups you set up in **Settings** cover your sites, not the panel.

To back up the panel, run this on the server as `root`. You need a backup store first: [Backups](../looking-after-a-project/backups.md).

```bash
pae project:backup:create panel --container=<store>
pae project:backup:list panel
```

`<store>` is the name of your backup store. To roll the panel back, run `pae project:backup:restore panel <id>`. **A restore overwrites the panel's current data.**

## Removing the panel

**This deletes the panel project and all its data. There is no undo.** Your other projects are not touched.

1. Log in to your VPS as `root` over SSH.
2. Run this. `panel` is the project name you gave at install:

   ```bash
   pae project:delete panel
   ```

3. Confirm when `pae` asks.

To remove PanelAlpha Engine and every project on it, follow the engine's own removal steps. **That deletes every site.**

## Getting the panel back

Install again: [Installing the panel](install.md). **A new panel always starts empty**, with no accounts and no settings. Deleting the panel project also deletes its backups, so you cannot restore the old panel into a new install.
