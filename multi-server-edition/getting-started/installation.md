---
title: Installation
description: Install PanelAlpha on Debian or Ubuntu, run the setup wizard, and configure your first server.
nav:
  group: Getting Started
  order: 1
  title: Installation Guide
---

# Installation

This article walks you through installing PanelAlpha on your server.

## System Requirements

To install and use PanelAlpha, the following conditions must be fulfilled:

1. **Server for PanelAlpha installation**:
   - **Operating system:**
     - **Debian 12 "bookworm"** or
     - **Debian 13 "trixie"** or
     - **Ubuntu 24.04 LTS (Noble Numbat)** or
     - **Ubuntu 22.04.4 LTS (Jammy Jellyfish)**
   - **Memory:** We recommend **4GB of RAM**
   - **Open inbound ports:**
     - **80** and **443** (web traffic)
     - **8443** (PanelAlpha admin area)

2. **Connection to a hosting server** — choose one:
   - **cPanel, DirectAdmin, or Plesk server** — for Shared Hosting control panel *(root access required)*
   - **Dedicated server/VPS** — to be used with [PanelAlpha Engine](../../panelalpha-engine/index.md)

   **Note:** The engine cannot be installed on the same server as PanelAlpha.

## Before You Begin

1. Ensure your server meets all system requirements.
2. Make sure you have root access via SSH.
3. Have your PanelAlpha license key ready. Find it in your client area under [Licenses](https://manage.panelalpha.com/clientarea.php?action=services).
4. Ensure your server has internet connectivity.
5. Confirm your firewall allows inbound traffic on **80/443/8443**.

## Download and Install

1. **Log in to your server** via SSH as root:
   ```bash
   ssh root@your-server-ip
   ```

2. **Run the installation script**:
   ```bash
   curl -fsSL https://get.panelalpha.com/app | sh
   ```

   **Note:** The same script is used for both installation and updates. It automatically detects whether PanelAlpha is already installed and performs the appropriate action.

## Initial Configuration

Once installation is complete, proceed with initial configuration using the Setup Wizard.

### Access the Wizard

The Setup Wizard can be accessed at:
```
https://YOUR_SERVER_IP:8443
```

Replace `YOUR_SERVER_IP` with your actual server IP address.

### Configuration Process

The Setup Wizard is designed for non-technical users. Typical flow:

- **General details** — Set company/application name and create the first admin user.
- **Branding** — Optionally upload a logo and set colors.
- **Plans** — Define at least one plan for provisioning instances.
- **First server** — Add your hosting server (cPanel, DirectAdmin, Plesk, or PanelAlpha Engine).
- **First service/instance** — Create the first service to confirm the connection works.

You can reopen the wizard later at `https://YOUR_SERVER_IP:8443/wizard`.

## Next Steps

After completing installation and initial configuration:

1. **Configure hosting servers** — Connect cPanel, DirectAdmin, Plesk, or Docker-based servers
2. **Set up billing integration** — Configure your preferred billing system
3. **Review security settings** — Ensure proper SSL and security measures
4. **Test the installation** — Verify all components are working correctly

## Common Problems

If you encounter issues, refer to the [Troubleshooting Center](../../troubleshooting-center/index.md) for common problems and solutions.
