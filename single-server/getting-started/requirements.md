---
title: "Requirements"
description: "Check your VPS against the list below before you install PanelAlpha Single Server Edition. The installer sets up the Engine and the panel for you."
nav:
  group: Getting Started
  order: 1
---

# Requirements

Check your VPS against the list below before you install. The installer sets up the PanelAlpha Engine and the panel for you: [Installing the panel](install.md).

## The VPS

You need a VPS that meets all five:

| Requirement | What you need |
|---|---|
| **Operating system** | Debian 12 or 13, or Ubuntu 22.04, 24.04 or 26.04 |
| **Access** | Logged in as `root` over SSH, not `sudo` |
| **The VPS** | Brand new, with nothing else running on it |
| **Memory** | 2 GB of RAM, more as you add projects |
| **Network** | A public IPv4 address |

The installer checks the operating system and refuses anything else. It does not check the rest, so check them yourself.

> [!NOTE]
> **Why the VPS has to be brand new.** The installer takes the machine over: it installs Docker, a firewall and a watchdog, and it changes how the machine looks up domain names. That is fine on an empty VPS and destructive on one already doing a job. Do not use a VPS that already runs a website or another control panel.

## Ports

**Port 80 must be open to everyone.** Let's Encrypt uses it to issue and renew certificates.

The installer opens these ports in the server's own firewall:

- 22 for SSH.
- 80 and 443 for websites and for the panel.
- 2011 for the Engine API. It can stay closed to the internet.
- 21 and 30000 to 30009 for FTP.
- 2222 for SFTP.

**Your provider may put a firewall in front of the VPS**, for example Hetzner Cloud Firewalls or AWS security groups. The installer cannot see or change that firewall, so open the same ports there yourself.

## A domain

Use a domain you own for the panel, for example `panel.example.com`. Before you install, point its A record at the server's IP address (`203.0.113.10`). The panel works over HTTPS only, and the domain gets a trusted Let's Encrypt certificate once it points at the server.

Which names the panel can use, and which of them get a trusted certificate: [The panel address](panel-address.md).

Next: [Installing the panel](install.md).
