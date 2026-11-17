---
title: Large WordPress Sites
description: Learn how PanelAlpha supports large WordPress sites through staging, backups, imports, and hosting optimizations.
nav:
  group: System Configuration
  order: 3
---

# Large WordPress Sites

## Overview

This page explains how PanelAlpha supports **large or high-traffic WordPress sites**, and how those capabilities map to real operational workflows (staging, push, backups, restores, imports, migrations, updates).

"Large" does not only mean big in GB. Large instances are often "big in operations", for example:

- Many files/inodes (especially `wp-content/uploads`)
- A large or fast-growing database (common for WooCommerce, membership sites, LMS)
- High concurrency (traffic spikes) and long-running background jobs

For large sites, the key goal is to reduce risk during common high-impact operations:

- Creating staging and pushing changes
- Running backups and restores
- Importing and migrating instances
- Applying updates and troubleshooting incidents

## Before You Begin

1. Decide which category fits your situation:
   - Performance under peak load (timeouts, slow admin)
   - Operational safety (staging, restore time, change windows)
   - Migration risk (imports, cutovers)
2. Check the two main size drivers:
   - Files: WordPress directory size and file count (especially uploads)
   - Database: size, growth rate, and write frequency
3. For high-write sites (WooCommerce/membership), plan a short content freeze window for risky cutovers (push full DB, restores, migrations).

## Hosting and Plan Capabilities

Large sites require the right hosting foundation and plan-level controls.

PanelAlpha supports multiple hosting server types and plan configuration options:

- Choose a server type suited for isolation and scalability (for example PanelAlpha Engine or WP Cloud).
- Configure plan-level performance knobs (for example PHP workers on WP Cloud).
- Enable platform-supported caching options (for example Redis and/or LiteSpeed cache).

References:

- [PanelAlpha Engine](../hosting-servers/engine.md)
- [WP Cloud](../hosting-servers/wp-cloud.md)
- [Plans](plans.md)

**Warning:**
- Some improvements require plan changes, which can affect how new instances are provisioned.
- Apply changes during low-traffic hours when possible.

## Staging and Push

PanelAlpha supports a staging workflow for controlled change management:

- Create staging for an instance.
- Pre-check disk usage before creating staging (useful for large file trees).
- Push changes in both directions:
  - Push staging to live
  - Push live back to staging

To reduce risk on large sites, PanelAlpha supports fine-grained push options:

- Overwrite files
- Push full database
- Push selected tables with structural changes
- Push selected tables with changed data

PanelAlpha also supports previewing detected database changes between live and staging before you push.

Client Area reference: [Instance Details](../client-area-instances/instance-details.md)

Operational notes for large sites:

- Avoid pushing a full database during peak traffic on high-write sites unless you have a freeze window.
- Prefer table-level push options when the database is large or continuously changing.

## Backups and Restores

PanelAlpha supports per-instance backups in the Client Area:

- List backup history
- Create manual backups
- Configure automatic backup schedules (availability depends on plan/product)
- Download backups
- Restore backups
- Delete backups

Backup scope:

- Files (directory)
- Database
- "Full backup" typically means files + database

Restore capabilities useful for large sites:

- Restore files only or database only (depending on what the backup contains)
- Option to delete existing files before restoring (when applicable)

Client Area reference: [Backups](../client-area-instances/backups.md)

**Suggestion:** Consider a server-level backup layer (for example JetBackups) alongside PanelAlpha backups for longer retention or provider-specific restore workflows.

**Important:**
- JetBackups is a separate third-party system and is not required for PanelAlpha.
- Your restore procedure depends on how your hosting provider implements backups.

## Imports, Copy, and Migrations

PanelAlpha supports multiple ways to move or duplicate workloads:

- Client-triggered import ("import an existing WordPress")
- Copying an instance to another service
- Admin-managed migration center for visibility and control

For large sites, choose a workflow that matches risk:

- Self-service import when the client can provide source access and downtime expectations are clear.
- Admin-managed migrations when you need progress visibility and controlled execution.

References:

- [Importing Existing Instance](../client-area-instances/importing-existing-instance.md)
- [Migrations](../admin-area/migrations.md)

## Updates, Maintenance, and Troubleshooting

For large sites, planned and repeatable changes matter. PanelAlpha supports:

- One-click updates (WordPress core and related update flows)
- Auto-update configuration (where enabled)
- Maintenance mode and debug mode controls
- Cache clear operation (when supported by the hosting/integration)

For incident response and diagnostics, PanelAlpha provides instance-level visibility including:

- Activity logs
- Webserver logs
- PHP logs

These capabilities help correlate slowdowns and timeouts with traffic spikes, cron-heavy plugins, or deployments.

## Access and Collaboration

For teams working on large instances, PanelAlpha supports:

- Secure WordPress admin access via SSO (when enabled)
- Shared access and collaboration features for instances

## Reporting and Analytics

PanelAlpha provides instance-level reporting useful for large or high-traffic sites:

- Bandwidth reporting (by period)
- Visitors reporting (by period)

## Edge Cache (WP Cloud) and CDN Controls

For large sites, cache and CDN controls can reduce origin load during spikes.

PanelAlpha supports WordPress edge-cache management:

- Viewing current edge cache configuration
- Updating edge cache settings
- Purging edge cache
- Toggling defensive mode

If the instance is connected to Cloudflare, PanelAlpha also supports:

- Viewing and updating security level
- Viewing and toggling Browser Integrity Check
- Viewing and toggling Development Mode
- Purging Cloudflare cache

### Edge Cache and Cloudflare Proxy Conflict

Running both Edge Cache and Cloudflare Proxy simultaneously can cause caching conflicts — two independent cache layers may serve stale or inconsistent content.

PanelAlpha warns about this in two places:

**When enabling Edge Cache** — if the instance domain uses Cloudflare DNS with proxy active, toggling Edge Cache on does not apply immediately. A warning modal appears explaining the conflict. You can choose **Enable Anyway** to proceed, or cancel. Once Edge Cache is enabled alongside Cloudflare Proxy, a persistent inline warning is displayed under the Edge Cache switch as a reminder.

**When enabling Cloudflare Proxy** — in the [DNS Zone Editor](../client-area-hosting/dns-zone-editor.md), toggling the proxy on for a record shows a warning modal if Edge Cache is already active on a WordPress instance using that domain.
