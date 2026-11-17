---
title: "Installing the panel"
description: "One command as root on a fresh VPS installs the PanelAlpha Engine and the panel. Check the requirements first, then run it and open the panel address."
nav:
  group: Getting Started
  order: 2
---

# Installing the panel

One command as `root` on a fresh VPS installs the PanelAlpha Engine and the panel. Check your VPS first: [Requirements](requirements.md).

## Installing

1. Log in to your VPS as `root` over SSH.
2. Run:

   ```bash
   curl -fsSL https://get.panelalpha.com/single-server | sh
   ```

   It takes several minutes. When it finishes, it prints the address of your panel.

3. Open that address in your browser. The setup wizard creates your account: [First login](first-login.md).
