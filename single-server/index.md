---
title: "PanelAlpha Single Server Edition"
description: "PanelAlpha Single Server Edition is a web hosting panel you install on your own VPS. You open it in a browser and look after your sites there. The Engine..."
nav:
  group: Single Server
  order: 1
  title: Introduction
section:
  title: Single Server
  order: 1
  groups:
    - title: Single Server
      path: index.md
      collapsable: false
    - title: Getting Started
    - title: Projects
    - title: Looking After a Project
    - title: The Panel
    - title: Editions
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

PanelAlpha Single Server Edition is a web hosting panel you install on your own VPS. You open it in a browser and look after your sites there. The Engine does the hosting underneath, and the panel gives you a screen for it.

The free edition is for one person and has no project limit. In the panel you can:

- Create a WordPress site, or deploy an app from Git or from files
- Import a WordPress site from another host
- Add domains, with certificates from Let's Encrypt
- Take backups by hand or on a schedule, and restore them
- Keep a staging copy of a site and push it live
- Manage databases, cron jobs, FTP and SFTP accounts and PHP settings
- See visitor statistics, usage and logs
- Connect an AI assistant and manage your projects by chat
- Connect the server to your PanelAlpha Hub account and sign in with it

Setting it up takes three steps: [Getting started](#getting-started).

## How it works

![How the panel fits on your server: you and your AI assistant reach the panel through nginx-proxy; the panel asks the Engine API, which runs every site in its own container; you reach the engine over SSH with pae; visitors reach the sites through nginx-proxy; the panel checks the licence server, the engine gets certificates from Let's Encrypt and writes backups to a store](../assets/single-server/architecture.gif)

The panel is one project on the Engine, in its own container. When you create a site, the panel asks the Engine API to do it, and the Engine gives that site a project of its own. One panel manages the Engine it runs on.

Removing the panel deletes only the panel's project. Your sites keep running: [Removing the panel](getting-started/removing.md).

## Getting started

### 1. Install the panel on your VPS

You need a fresh Debian 12/13 or Ubuntu 22.04/24.04/26.04 server with 2 GB of RAM, `root` access over SSH and a domain pointed at it. **Port 80 must be open to everyone**, because Let's Encrypt uses it.

```bash
curl -fsSL get.panelalpha.com/single-server | sh
```

Every option and the Engine version the panel needs: [Installing the panel](getting-started/install.md).

### 2. Sign in

Open `https://panel.example.com`. The setup wizard creates your account, or connects your PanelAlpha Hub account, and opens the panel: [First login](getting-started/first-login.md).

### 3. Create your first site

Go to **My Projects**, select **Create First Project** and choose **WordPress**, **Git** or **Files**. You can also drop a `.zip` or `.tar.gz` archive straight onto the project list: [Create a project](projects/create-a-project.md).

## How to use it

- **[Getting started](getting-started/requirements.md)**: [Requirements](getting-started/requirements.md), [installing the panel](getting-started/install.md), [first login](getting-started/first-login.md), [the panel address](getting-started/panel-address.md), [updating](getting-started/updating.md) and [removing](getting-started/removing.md) the panel.
- **[Projects](projects/projects.md)**: [Creating a project](projects/create-a-project.md), [managing WordPress](projects/wordpress.md), [importing a WordPress site](projects/importing-wordpress.md), [Git](projects/git.md) and [staging and preview links](projects/staging-and-preview.md).
- **[Looking after a project](looking-after-a-project/domains-and-ssl.md)**: [Domains and SSL](looking-after-a-project/domains-and-ssl.md), [databases](looking-after-a-project/databases.md), [backups](looking-after-a-project/backups.md), [files, FTP and SFTP](looking-after-a-project/files-and-access.md), [cron jobs and PHP](looking-after-a-project/cron-and-php.md) and [statistics and logs](looking-after-a-project/statistics-and-logs.md).
- **[The panel](the-panel/settings.md)**: [Settings](the-panel/settings.md), [tasks](the-panel/tasks.md), [your account](the-panel/your-account.md), [Connect AI](the-panel/connect-ai.md) and [around the panel](the-panel/around-the-panel.md).
- **[Editions](editions/editions-and-licence.md)**: [Editions and licence](editions/editions-and-licence.md), [the Admin Area](editions/admin-area.md) and [sharing projects](editions/sharing-projects.md).

## Editions

| Edition | People | Admin Area | Licence key |
|---|---|---|---|
| Single Server Free | one | no | not needed |
| Single Server Plus | one default user | yes | yes |
| Single Server Pro | many, with shared access to projects | yes | yes |

What each one allows and what the limits are: [Editions and licence](editions/editions-and-licence.md).
