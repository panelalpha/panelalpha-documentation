---
title: "Updating the panel"
description: "You update PanelAlpha Single Server Edition by deploying a new archive into the panel's project with pae on the server. A panel from the one-line installer..."
nav:
  group: Getting Started
  order: 5
---

# Updating the panel

You update PanelAlpha Single Server Edition by deploying a new archive into the panel's project with `pae` on the server. Your accounts, settings and sessions stay.

The panel has no update button and no update check. A panel from the one-line installer is updated by running the installer again: [A panel from the one-line installer](#a-panel-from-the-one-line-installer).

If you protected the project at install, no API token can redeploy it, the panel's own included. Only `pae` on the server can, so you update there. The panel's own project is not in its project list: [Installing the panel](install.md).

## Updating

1. Download the new archive to `/opt/panelalpha`, as at install:

   ```bash
   curl -fsSL -o /opt/panelalpha/panel-new.tar.gz \
     https://github.com/panelalpha/panelalpha-single-server/archive/refs/heads/master.tar.gz
   ```
2. Deploy it into the panel's project:

   ```bash
   pae project:deploy-archive --project panel --file /opt/panelalpha/panel-new.tar.gz
   ```

   If another task of the project is still running, for example a deploy you started earlier, the command refuses with a message that names that task, starting `Project 'panel' is busy:`. Wait a few seconds and run the command again.

3. Wait for the command to finish, then open the panel and sign in.

The panel's data lives in one volume that a deploy keeps: [Removing the panel](removing.md). When the new version starts it updates its database by itself.

## What a deploy keeps

You do not have to unprotect the project. `pae project:rebuild` takes no variables and keeps them too.

The variables you passed at install (`PANELALPHA_USER_EMAIL`, `PANELALPHA_USER_PASSWORD`, `PANELALPHA_SELF_PROJECT`) are stored with the project and kept on every deploy. You do not pass them again. A variable you pass with `--env-var` on a later deploy is added to them or replaces the one with the same name.

## Updating the Engine

The Engine is updated separately and your projects keep running. The panel needs Engine 2.3.0 or later: [Requirements](requirements.md).

## A panel from the one-line installer

A panel the one-line installer put on the server deploys from its Git repository, so `pae project:deploy-archive` refuses it. Run the one-line installer again. It finds the panel it made and deploys it again from its repository instead of adding a second one. The panel's name, domain, settings, data and variables stay.

When a newer Engine is available, the installer updates only the panel and says so. Add `--update-engine` to update the Engine as well:

```bash
curl -fsSL get.panelalpha.com/single-server | sh -s -- --update-engine
```
