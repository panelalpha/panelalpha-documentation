---
title: Installation
description: Step-by-step guide to installing PanelAlpha Single Server Edition on Debian or Ubuntu.
nav:
  group: Getting Started
  order: 1
---

# Installation

This guide walks you through installing PanelAlpha on one server.
Typical install time is about **10-25 minutes** (depending on server performance and network speed).

## Requirements

- Debian `12/13` or Ubuntu `22.04/24.04`
- Root SSH access
- Minimum `4 GB RAM`
- Minimum `20 GB` disk space
- Open inbound ports: `80`, `443`, `8443`, `8444`
- Optional inbound ports (for FTP/SFTP access): `21`, `30000-30009`, `2222`
- A clean server (recommended)

## Installation Steps

1. **Log in to your server** via SSH as root:
   ```bash
   ssh root@your-server-ip
   ```

2. **Run the following command to start installation**:
   ```bash
   wget -N -P /opt/panelalpha https://license.panelalpha.com/single-server-installer.sh && bash /opt/panelalpha/single-server-installer.sh
   ```

3. **Wait for completion**. Do not close the SSH session while the installer is running.

## First Access

After installation finishes:

1. Open the **Admin Area** at `https://YOUR_SERVER_IP:8443`.
2. Complete the first configuration wizard.
3. Open the **Client Area** at `https://YOUR_SERVER_IP:8444`.

If your browser shows a certificate warning on first access, proceed to the site and then configure SSL in [SSL Configuration](ssl-configuration.md).

![PanelAlpha Single Server — Getting Started — First configuration wizard after installation](../../assets/img/SSE/Admin-Area/Wizard_1.png)

## Need Help?

If installation fails or one of the URLs does not open, check the [Troubleshooting Center](../../troubleshooting-center/index.md).

## Next Steps

1. [Admin Area](../admin-area/index.md)
2. [Client Area Instances](../client-area-instances/index.md)
3. [Client Area Hosting](../client-area-hosting/index.md)
