---
title: "Updating the panel"
description: "You update the panel by running the installer again on the server. Your accounts, settings, data and domain stay."
nav:
  group: Getting Started
  order: 6
---

# Updating the panel

You update the panel by running the installer again on your VPS. Your accounts, settings, data and domain stay. The panel has no update button and no update check.

## Updating

1. Log in to your VPS as `root` over SSH.
2. Run the installer again:

   ```bash
   curl -fsSL https://get.panelalpha.com/single-server | sh
   ```

   It finds the panel it installed and updates it. It does not add a second panel.

3. When it finishes, open the panel and sign in.

## Updating PanelAlpha Engine too

When a newer PanelAlpha Engine is available, the installer updates only the panel and says so. Add `--update-engine` to update the engine as well:

```bash
curl -fsSL https://get.panelalpha.com/single-server | sh -s -- --update-engine
```

Your projects keep running during the update.
