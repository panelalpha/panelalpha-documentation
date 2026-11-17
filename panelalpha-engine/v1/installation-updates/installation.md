---
title: Installation
description: Step-by-step guide to installing PanelAlpha Engine via SSH. Download the installer, run with your license key, and connect through the wizard.
nav:
  group: Installation & Updates
  order: 1
---

# Installation

This page explains how to install PanelAlpha Engine 1.x on a server and connect it to PanelAlpha Single Server or Multi Server.

## Before You Begin

1. Use a fresh server that meets the [system requirements](../index.md#system-requirements). Do not install the Engine on the server that runs PanelAlpha.
2. Make sure you can log in to the server over SSH as `root`.
3. Have your PanelAlpha license key ready. It is in the product details under [Licenses](https://manage.panelalpha.com/clientarea.php?action=services) in the client area.
4. Make sure the server can reach the internet. The installer downloads packages and Docker images.

## Download and Install

The PanelAlpha installation wizard shows the same commands. See [Access the Wizard](../../../multi-server/getting-started/installation.md#access-the-wizard).

1. Log in to the server as root:
   ```bash
   ssh root@your-server-ip
   ```

2. Download the installer:
   ```bash
   wget -N -P /opt/panelalpha https://license.panelalpha.com/engine-installer.sh
   ```

3. Run the installer with your license key:
   ```bash
   bash /opt/panelalpha/engine-installer.sh --key 'YOUR_LICENSE_KEY'
   ```

   Replace `YOUR_LICENSE_KEY` with your key from the client area.

4. Wait for the installer to finish. At the end it prints the **API URL** and the **API Token**. Copy both. You need them to add the server in PanelAlpha.

The installer sets up Docker, the Engine containers, the CSF firewall, monit and some kernel settings. If the server has a public IP address, it also gets a Let's Encrypt certificate for that address. The Engine is installed in `/opt/panelalpha/shared-hosting`.

### Installer Options

| Option | What it does |
| --- | --- |
| `--key <key>` | Your PanelAlpha license key. Required. |
| `--version <version>` | Installs a given Engine version instead of the latest. |
| `--no-local-ip` | If the server's own address is private, uses the public IPv4 address (looked up online) for the API URL and the default IP. |
| `--enable-nat` | For servers behind 1:1 NAT, for example on some cloud providers. The Engine looks up the public IPv4 address for each private address. |
| `--debug` | Prints more output. Use it when you send the output to support. |

### Lost API Token

If you did not copy the token, create a new one on the Engine server:

```bash
docker compose -f /opt/panelalpha/shared-hosting/docker-compose.yml exec -T core php artisan api-tokens:create panelalpha --short
```

See [Artisan Commands](../command-reference.md) for listing and deleting tokens.

## Connect Through the PanelAlpha Installation Wizard

If you install PanelAlpha and PanelAlpha Engine together, you can add the Engine in step 3 of the PanelAlpha wizard, **Add your first server**.

![PanelAlpha Engine — Hosting Server Connections — Add PanelAlpha Engine server in installation wizard](../../../assets/img/hosting-server-connections/PanelAlpha-Engine/PAE_1.png)

1. Select **PanelAlpha Engine** as the server type.
2. Click **Install PanelAlpha Engine** to see the installer commands, and run them on the Engine server as described above.
3. Paste the **API URL** and the **API Token** from the installer output.
4. Click **Test Connection**. When it shows **Finished**, continue to the next step.

## Next Steps

1. [Add the server](../../../multi-server/hosting-servers/engine.md#configure-in-panelalpha) in PanelAlpha, if you did not do it in the wizard.
2. [Set up a plan](../../../multi-server/system-configuration/plans.md#hosting-server) that uses the server.
3. Check the [Firewall (CSF)](../server-management/firewall.md) rules, and set up [Outgoing Mail](../server-management/outgoing-mail.md) so sites can send email.

## Troubleshooting

If the installer stops with an error, see [Troubleshooting](../troubleshooting.md#installation-fails).
