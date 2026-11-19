---
title: Update
description: Update PanelAlpha using the updater script, review requirements, and troubleshoot common issues.
nav:
  group: Getting Started
  order: 2
  title: Update Guide
---

# Update

This article explains how to successfully update PanelAlpha.

## System Requirements

Before updating, ensure your server meets current requirements:

1. **Operating system:**
   - **Debian 12 "bookworm"** or
   - **Debian 13 "trixie"** or
   - **Ubuntu 24.04 LTS (Noble Numbat)** or
   - **Ubuntu 22.04.4 LTS (Jammy Jellyfish)**
2. **Memory:** We recommend **4GB of RAM**
3. **Root access** via SSH

## Before You Begin

1. **Create a complete backup** of your server and database. This is essential to prevent data loss.
2. Ensure you have your PanelAlpha license key ready. Find it in your client area under [Licenses](https://manage.panelalpha.com/clientarea.php?action=services).
3. Check the [changelog](https://panelalpha.com/changelog) for compatibility and breaking changes.
4. Test the update process on a staging environment first, if available.
5. Ensure you have root access via SSH.

## Update PanelAlpha Application

The following steps guide you through updating PanelAlpha for maintenance releases:

1. **Access your server** via SSH with root privileges:
   ```bash
   ssh root@your-server-ip
   ```

2. **Run the update script**:
   ```bash
   curl -fsSL https://get.panelalpha.com/app | sh
   ```

   **Note:** The same script is used for both installation and updates. It automatically detects whether PanelAlpha is already installed and updates it when needed.

3. **Wait for the update to complete** and follow any on-screen instructions.

## Common Problems

If you encounter issues, refer to the [Troubleshooting Center](../../troubleshooting-center/index.md) for common problems and solutions.

For additional support, contact our [support team](https://manage.panelalpha.com/submitticket.php?step=2&deptid=1).
