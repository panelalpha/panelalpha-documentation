---
title: "Updating the panel"
description: "You update the panel by running the installer again on the server. Your accounts, settings, data and domain stay."
nav:
  group: Getting Started
  order: 5
---

# Updating the panel

You update the panel by running the installer again on the server. Your accounts, settings, data and domain stay. The panel has no update button and no update check.

## Updating

1. Log in to your VPS as `root` over SSH.
2. Run the installer again:

   ```bash
   curl -fsSL https://get.panelalpha.com/single-server | sh
   ```

   It finds the panel it installed and deploys it again from its repository. It does not add a second panel. The panel keeps its name, domain, settings, data and variables.

3. When it finishes, open the panel and sign in.

When the new version starts, it updates its database by itself. The panel's data lives in one volume that the update keeps: [Removing the panel](removing.md).

## Updating the Engine too

When a newer Engine is available, the installer updates only the panel and says so. Add `--update-engine` to update the Engine as well:

```bash
curl -fsSL https://get.panelalpha.com/single-server | sh -s -- --update-engine
```

Your projects keep running while the Engine is updated.
