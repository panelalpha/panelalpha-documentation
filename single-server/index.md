---
title: Single Server Edition
description: Overview of PanelAlpha Single Server Edition features, requirements, and documentation sections.
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
    - title: Client Area Instances
      path: client-area-instances/index.md
    - title: Client Area Hosting
      path: client-area-hosting/index.md
    - title: Admin Area
      path: admin-area/index.md
    - title: Configuration
      path: admin-area/default-plan.md
card:
  title: Single Server Edition
  description: Deploy and manage PanelAlpha on a single Linux server with a streamlined operational model.
  order: 1
---


# Single Server Edition

Welcome to the **PanelAlpha Single Server Edition** documentation. Single Server Edition is designed for teams that manage WordPress hosting on one Linux server, combining a client-facing interface (**Client Area**) with administrator tooling (**Admin Area**) in one deployment so you can provision WordPress sites, manage hosting resources, monitor activity, and control platform-level policies without switching between separate systems.

## Key Features

- Fast WordPress provisioning and import workflows
- Instance operations: staging, local backups, monitoring, plugins, and advanced settings
- Hosting toolkit: domains, FTP/SFTP, MySQL, cron jobs, and file manager
- Admin controls for users, services, plans, migrations, logs, and notifications
- Integration support for SSL automation, analytics, preview URLs, and more

## System Requirements

- Debian `12/13` or Ubuntu `22.04/24.04`
- Root SSH access
- Minimum `4 GB RAM`
- Minimum `20 GB` disk space
- Open inbound ports: `80`, `443`, `8443`, `8444`
- Optional inbound ports (for FTP/SFTP access): `21`, `30000-30009`, `2222`

## General Questions

### Is this edition really limited to one server?
Yes. Single Server Edition is intended for one server environment.

### Which URL is for clients and which is for admins?
- Client Area: `https://YOUR_DOMAIN_OR_IP:8444`
- Admin Area: `https://YOUR_DOMAIN_OR_IP:8443`

### Can I import existing WordPress websites?
Yes. See [Importing Existing Instance](client-area-instances/importing-existing-instance.md).

### Can I add more users or services?
No. The platform uses the default owner account (`#1`) and default service (`#1`) as the standard operating model.

### Where do I start if something fails?
Start with [Troubleshooting Center](../troubleshooting-center/index.md).

## Documentation Sections

- **[Getting Started](getting-started/index.md)** — Installation, first access, SSL, updates, and language setup.
- **[Client Area Instances](client-area-instances/index.md)** — WordPress instance operations: creation, import, maintenance, and monitoring.
- **[Client Area Hosting](client-area-hosting/index.md)** — Hosting operations: usage, domains, files, databases, access, and automation.
- **[Admin Area](admin-area/index.md)** — Platform administration: users, services, logs, migrations, configuration, and notifications.
