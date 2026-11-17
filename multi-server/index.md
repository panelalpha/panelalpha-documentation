---
title: Multi Server Edition
description: Centralized WordPress management across multiple hosting servers with client and admin workflows.
nav:
  group: Multi Server
  order: 1
  title: Introduction
section:
  title: Multi Server
  order: 2
  groups:
    - title: Multi Server
      path: index.md
      collapsable: false
    - title: Getting Started
      path: getting-started/index.md
    - title: System Configuration
      path: system-configuration/index.md
    - title: Migrations
      path: migrations/index.md
    - title: WP Cloud Migration
      path: wp-cloud-migration/index.md
    - title: Admin Area
      path: admin-area/index.md
    - title: Onboarding Methods
      path: onboarding-methods/index.md
    - title: Hosting Servers
      path: hosting-servers/index.md
    - title: DNS Servers
      path: dns-servers/index.md
    - title: Email Servers
      path: email-servers/index.md
    - title: Billing Systems Integrations
      path: billing-systems-integrations/index.md
    - title: Billing Scenarios
      path: billing-scenarios/index.md
    - title: Integrations
      path: integrations/index.md
    - title: Client Area - Instances
      path: client-area-instances/index.md
    - title: Client Area - Hosting
      path: client-area-hosting/index.md
card:
  title: Multi Server Edition
  description: Run PanelAlpha in a scalable multi-server environment with advanced integrations and hosting scenarios.
  tag: Many servers
  start:
    - getting-started/installation.md
    - system-configuration/general-configuration.md
    - hosting-servers/hosting-scenarios.md
    - billing-systems-integrations/whmcs.md
  order: 2
---


# Multi Server Edition

Welcome to the **PanelAlpha Multi Server Edition** documentation. Multi Server Edition is designed for companies that run WordPress hosting across multiple servers and external services, combining a client-facing interface (**Client Area**) with centralized administrator tooling (**Admin Area**) so you can connect hosting servers, automate onboarding, manage migrations, configure plans, and integrate DNS, email, billing, SSL, analytics, and other supporting services across your infrastructure.

## Key Features

- Centralized WordPress provisioning and management across multiple hosting servers
- Support for cPanel, DirectAdmin, Plesk, PanelAlpha Engine, and WP Cloud scenarios
- Client and admin workflows for instances, hosting resources, and migrations
- Flexible plans, onboarding methods, reseller options, and billing integrations
- Integration support for DNS, email, SSL automation, analytics, preview URLs, and more

## System Requirements

- Debian `12/13` or Ubuntu `22.04/24.04` for the PanelAlpha server
- Root SSH access
- Minimum `4 GB RAM`
- Open inbound ports: `80`, `443`, `8443`
- Connection to at least one supported hosting server
- If using PanelAlpha Engine, deploy it on a separate server

## General Questions

### Is this edition intended for multiple servers?
Yes. Multi Server Edition is built for centralized management of one or more external hosting servers and related services.

### Which hosting environments are supported?
PanelAlpha supports cPanel, DirectAdmin, Plesk, PanelAlpha Engine, and WP Cloud. See [Hosting Servers](hosting-servers/index.md).

### Do I need a billing integration to use it?
Not always. You can integrate with WHMCS or [Stripe](direct-billing/stripe.md) through [Direct Billing](direct-billing/index.md), depending on your setup.

### Can I import existing WordPress websites?
Yes. See [Importing Existing Instance](client-area-instances/importing-existing-instance.md) and [Migrations](migrations/index.md).

### Where do I start if something fails?
Start with [Troubleshooting Center](../troubleshooting-center/index.md).

## Documentation Sections

- **[Getting Started](getting-started/installation.md)** — Installation, SSL, updates, and translations.
- **[Client Area Instances](client-area-instances/list-of-instances.md)** — WordPress instance creation, import, backups, monitoring, plugins, sharing, and advanced settings.
- **[Client Area Hosting](client-area-hosting/summary.md)** — Hosting operations: domains, DNS zones, files, databases, access, cron jobs, and email management.
- **[Admin Area](admin-area/dashboard.md)** — Central administration for dashboard, users, services, instances, logs, and migrations.
- **[System Configuration](system-configuration/general-configuration.md)** — Global settings for plans, notifications, hosting servers, DNS, email, backups, billing, and automation.
- **[Hosting Servers](hosting-servers/hosting-scenarios.md)** — Supported hosting backends and deployment scenarios.
- **[DNS Servers](dns-servers/cloudflare.md)** — DNS provider integrations such as Cloudflare, Bunny.net, cPanel DNSOnly, and PowerDNS.
- **[Email Servers](email-servers/mailcow.md)** — Email server integrations such as Mailcow and cPanel.
- **[Billing Systems Integrations](billing-systems-integrations/whmcs.md)** — Stripe Direct Billing and WHMCS.
- **[Billing Scenarios](billing-scenarios/introduction.md)** — Billing models, onboarding flows, and related provisioning options.
- **[Onboarding Methods](onboarding-methods/quick-onboarding.md)** — Standard, quick, and super quick onboarding workflows.
- **[Migrations](migrations/index.md)** — Client-initiated imports, admin-managed migrations, troubleshooting, and best practices.
