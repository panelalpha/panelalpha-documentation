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

## Installing from PanelAlpha Hub

If you have a PanelAlpha Hub account, you can install from there. The panel then starts already connected to your Hub account, and you skip the setup wizard.

1. Sign in at [hub.panelalpha.com](https://hub.panelalpha.com).
2. Select **Add New**, then **Install on your own server**.
3. Choose the option with the free PanelAlpha panel and continue.
4. Copy the command the Hub shows. It holds your email address and a one-time code.
5. Log in to your VPS as `root` over SSH and run that command instead of the one above.

**The code works once and expires after 60 minutes.** If it has expired, get a new command in the Hub.

When the install finishes, it prints the panel address and a password for your Hub email, after `Log in as:`. Keep that password. Your Hub email is the panel's account, and you can sign in in three ways:

- **Sign in with PanelAlpha** on the panel's login page
- **Login** next to the server in the Hub
- your Hub email and the printed password

The Hub shows the new server once the panel sends its first report. What the connection does: [Settings](../the-panel/settings.md#panelalpha-hub).

If the Hub refuses the code, the panel is still installed, without the connection. Sign in with your email and the printed password, and connect later in **Settings**.
