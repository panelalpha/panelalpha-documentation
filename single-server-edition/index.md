---
title: "PanelAlpha Single Server Edition"
description: "PanelAlpha Single Server Edition is a web hosting panel you install on your own VPS. You open it in a browser and look after your sites there."
nav:
  group: Single Server Edition
  order: 1
  title: Introduction
section:
  title: Single Server Edition
  order: 1
  groups:
    - title: Single Server Edition
      path: index.md
      collapsable: false
    - title: Getting Started
    - title: Projects
    - title: Looking After a Project
    - title: The Panel
    - title: Troubleshooting
card:
  title: Single Server Edition
  description: A web hosting panel you install on your own VPS.
  tag: One server
  start:
    - getting-started/install.md
    - getting-started/first-login.md
    - projects/create-a-project.md
    - looking-after-a-project/domains-and-ssl.md
  order: 1
---

# PanelAlpha Single Server Edition

## What is PanelAlpha Single Server Edition?

PanelAlpha Single Server Edition is a web hosting panel you install on your own VPS (a server you rent). You open it in a browser and look after your sites there. **PanelAlpha Engine** is the hosting software underneath. The panel is the screen you use to control it.

The panel is free and open source, for one person, with no project limit. In the panel you can:

- Create a WordPress site, or deploy an app from Git or from files
- Install a ready app from the PanelAlpha GO! catalog
- Import a WordPress site from another host
- Add domains, with certificates from Let's Encrypt
- Take backups by hand or on a schedule, and restore them
- Keep a staging copy of a site and push it live
- Manage databases, cron jobs, FTP and SFTP accounts and PHP settings
- See visitor statistics, usage and logs
- Connect an AI assistant and manage your projects by chat
- Connect the server to your PanelAlpha Hub account and sign in with it

Setting it up takes three steps: [Getting started](#getting-started).

## PanelAlpha Hub and PanelAlpha GO!

The panel works on its own. You can also connect it to two free PanelAlpha services.

**PanelAlpha Hub** at [hub.panelalpha.com](https://hub.panelalpha.com) is your PanelAlpha account. There you see every server you connected, with its status, its sites and how much of the server they use. You also sign in to the panel from there, without a separate password: [Settings](the-panel/settings.md#panelalpha-hub).

**PanelAlpha GO!** at [go.panelalpha.com](https://go.panelalpha.com) is a catalog of open-source apps that PanelAlpha deployed and tested on PanelAlpha Engine. On the website, anyone can try an app for about an hour, with no account. In the panel, you install an app from the catalog as a project on your own server. The catalog shows in the panel once the panel is connected to PanelAlpha Hub: [Create a project](projects/create-a-project.md#creating-a-project-from-panelalpha-go).

## How it works

:docs-video{src="../assets/single-server-edition/architecture.webm" title="Diagram: you and visitors reach the panel and your sites on one VPS; the panel runs on PanelAlpha Engine"}

The panel is one project on PanelAlpha Engine. When you create a site, the panel asks the engine to do it, and the engine gives the site a project of its own.

Removing the panel deletes only the panel's project. Your sites keep running: [Removing the panel](getting-started/removing.md).

A short tour of the panel, from installing an app to the settings:

::docs-video{src="../assets/single-server-edition/panel-tour.webm" title="Tour of the panel: Add New and PanelAlpha GO!, the app Uptime Kuma installed, the Immich project with its details, monitoring, a new backup, usage limits and the Advanced pages, a staging copy pushed to live, Connect AI, My Server, Tasks, Settings with PanelAlpha Hub, and dark mode"}
::

## Getting started

### 1. Install the panel on your VPS

You need:

- A fresh Debian 12/13 or Ubuntu 22.04/24.04/26.04 server with 2 GB of RAM.
- `root` access over SSH.
- A domain pointed at the server.

**Port 80 must be open to everyone**, because Let's Encrypt uses it. Then run:

```bash
curl -fsSL https://get.panelalpha.com/single-server | sh
```

More: [Installing the panel](getting-started/install.md).

### 2. Sign in

Open `https://panel.example.com`. The setup wizard creates your account, or connects your PanelAlpha Hub account, and opens the panel. The wizard can also connect your AI assistant and ends by creating your first project: [First login](getting-started/first-login.md). Later visits start on the login screen: [Signing in](getting-started/signing-in.md).

### 3. Create your first site

Go to **My Projects**, select **Create First Project** and choose **WordPress**, **Git**, **Files** or **PanelAlpha GO!**. You can also drop a `.zip` or `.tar.gz` archive straight onto the project list: [Create a project](projects/create-a-project.md).
