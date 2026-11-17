---
title: "Requirements"
description: "PanelAlpha Single Server Edition needs a server that meets the Engine's requirements and runs Engine 2.3.0 or later. This page lists what to have ready..."
nav:
  group: Getting Started
  order: 1
---

# Requirements

PanelAlpha Single Server Edition needs a server that meets the requirements of the PanelAlpha Engine and runs Engine 2.3.0 or later. The panel is one of the Engine's projects, so the Engine's requirements apply.

## The server

- **Operating system:** Debian 12 or 13, or Ubuntu 22.04, 24.04 or 26.04.
- **Access:** you are logged in as `root` over SSH.
- **Memory:** 2 GB of RAM at the least, and more as you add projects.
- **Network:** a public IPv4 address.

The Engine installer checks only the operating system. The rest is up to you.

## The Engine

The panel needs PanelAlpha Engine 2.3.0 or later. An older Engine is not supported. To check the version, run this on the server:

```bash
pae system:version
```

The Engine installer takes the latest Engine published on GitHub. If `pae system:version` prints less than 2.3.0, the panel cannot be installed on that Engine:

```bash
curl --proto '=https' -fsSL https://get.panelalpha.com/engine | sh
```

One panel manages the Engine it runs on.

## Ports

**Port 80 must be open to everyone.** Let's Encrypt uses it to issue and renew certificates.

The Engine opens these ports in its own firewall:

- 22 for SSH.
- 80 and 443 for websites and for the panel.
- 2011 for the Engine API. It can stay closed to the internet.
- 21 and 30000 to 30009 for FTP.
- 2222 for SFTP.

If your hosting provider puts its own firewall in front of the server (Hetzner Cloud Firewalls or AWS security groups, for example), open the same ports there. The Engine cannot change that firewall.

## A domain

Use a domain you own, for example `panel.example.com`. Before you install, point its A record at the server's IP address (`203.0.113.10`). The panel is served over HTTPS only, so it needs a certificate.

## What you need on hand

- Root access to the server over SSH.
- The panel archive, a `.tar.gz` file. You download it from the public repository during the install: [Installing the panel](install.md).
- The email address and password for your first account. You can also leave these out and create the account in the browser: [First login](first-login.md).

## More detail: certificates

The Engine gets its own certificate from Let's Encrypt at install: for its own name when it has one, otherwise for the server's address. That certificate is trusted when the server has a public address and port 80 is open.

A name under `panelalpha.direct` that is not the server's own name keeps a self-signed certificate, and your browser shows a warning for it. A name of your own gets a trusted certificate: [The panel address](panel-address.md).

Next: [Installing the panel](install.md).
