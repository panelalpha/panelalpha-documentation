---
title: "Requirements"
description: "Check your VPS against the list below before you install PanelAlpha Single Server Edition. The installer sets up PanelAlpha Engine and the panel for you."
nav:
  group: Getting started
  order: 1
---

# Requirements

Check your VPS against the list below before you install. The installer sets up PanelAlpha Engine (the hosting software the panel runs on) and the panel for you: [Installing the panel](install.md).

## The VPS

You need a VPS that meets all five:

| Requirement | What you need |
|---|---|
| **Operating system** | Debian 12 or 13, or Ubuntu 22.04, 24.04 or 26.04 |
| **Access** | Logged in as `root` over SSH, not `sudo` |
| **The VPS** | Brand new, with nothing else running on it |
| **Memory** | 2 GB of RAM, more as you add projects |
| **Network** | A public IPv4 address |

The installer checks the operating system and refuses any other. It does not check the rest, so check them yourself.

> [!NOTE]
> **Use a brand new VPS.** The installer takes the machine over. It installs its own software and a firewall, and it changes how the machine looks up domain names. Do not use a VPS that already runs a website or another control panel.

## Ports

The installer opens the ports it needs in the VPS's own firewall. You do not open them yourself:

- 22 for SSH
- 80 and 443 for websites and the panel
- 2011 for the engine (it can stay closed to the internet)
- 21 and 30000 to 30009 for FTP
- 2222 for SFTP

**Port 80 must be open to everyone**, because Let's Encrypt uses it to issue and renew certificates.

**If your provider has its own firewall in front of the VPS** (for example Hetzner Cloud Firewalls or AWS security groups), open the same ports there by hand. The installer cannot change it.

## A domain

Use a domain you own for the panel, for example `panel.example.com`. Before you install, point its A record (the DNS entry that links a name to an IP address) at the server's IP address (`203.0.113.10`). The panel works over HTTPS only. The domain gets a trusted Let's Encrypt certificate once it points at the server.

Which names the panel can use: [The panel address](panel-address.md).

Next: [Installing the panel](install.md).
