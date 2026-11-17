---
title: Update
description: Safely update PanelAlpha Single Server Edition with pre-update checks and post-update validation.
nav:
  group: Getting Started
  order: 2
---

# Update

This guide explains how to safely update your PanelAlpha installation.

## Before You Start

1. Create a recent server backup or snapshot.
2. Make sure you have root SSH access.
3. Plan a short maintenance window (some panel functions can be briefly unavailable during update).

## Update Steps

1. **Log in to your server** via SSH as root:
   ```bash
   ssh root@your-server-ip
   ```

2. **Run the following command to start the update**:
   ```bash
   wget -N -P /opt/panelalpha https://license.panelalpha.com/single-server-installer.sh && bash /opt/panelalpha/single-server-installer.sh
   ```

The script auto-detects the existing installation, downloads the required bundle, and performs the update with no extra flags.
No manual downloads or license key arguments are required.

## After Update

1. Open `https://YOUR_SERVER_IP:8443` and confirm Admin Area loads correctly.
2. Open `https://YOUR_SERVER_IP:8444` and confirm Client Area loads correctly.
3. Check one real instance page (for example **Instance Details**) to confirm normal operation.
