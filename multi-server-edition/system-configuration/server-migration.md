---
title: Server Migration
description: Migrate PanelAlpha installations or WordPress instances between servers using the Snapshot Tool or import flows.
nav:
  group: System Configuration
  order: 16
---

# Server Migration

This article describes common migration scenarios:

1. Moving the **whole PanelAlpha installation** to a new machine.
2. Moving a **single WordPress instance** (or multiple instances) between **hosting servers**, while keeping PanelAlpha on the same machine.

## 1) Migrating the Whole PanelAlpha Installation (Snapshot Tool)

If you are moving PanelAlpha itself (containers, database, volumes, configuration) to a new server, use the **PanelAlpha Snapshot Tool**.

- Documentation: [Snapshot Tool](snapshot-tool.md)
- Repository: https://github.com/panelalpha/PanelAlpha-Snapshot-Tool/

The Snapshot Tool is designed for **full installation** backups and restores. If your goal is to migrate only a single WordPress website to another hosting server, skip to section 2.

**Warning:**
- Expect some downtime during the cutover.
- This procedure assumes you can move traffic (DNS / reverse proxy / firewall) from the old server to the new server.

### Before You Start

- Plan a short maintenance window. A full restore is a disruptive operation.
- Make sure the new server meets the PanelAlpha requirements (OS, dependencies, network access).
- Ensure you know where your snapshot repository is (S3/SFTP/local) and that you have the encryption password.

### Migrate PanelAlpha to the Target Server

#### 1) Prepare the target server

Set up the new machine (OS, networking, firewall rules, and any required dependencies). Make sure it can reach the snapshot repository (S3/SFTP/local).

#### 2) Create a snapshot on the source server

On the old server, create a snapshot using the Snapshot Tool.

#### 3) Restore the snapshot on the target server

Install the Snapshot Tool on the new server, connect it to the same repository, and restore the snapshot.

For exact commands and the restore sequence, follow: [Snapshot Tool – Migration to New Server](snapshot-tool.md#migration-to-new-server).

**Important:** Restoring a snapshot on the destination server will **replace the current PanelAlpha data** on that server.

#### 4) Cut over traffic

Point traffic to the new server (DNS / reverse proxy / firewall). If possible, lower DNS TTL ahead of time to make the cutover faster.

#### 5) Decommission the source server

Once the new server is stable and serving traffic, shut down or decommission the old server.

**Note:** If you are unsure whether anything still points to the old server, keep it online (restricted) for a short period while you validate.

### Finish Your Migration

After the cutover, verify:

- You can log in to both the Admin Area and Client Area.
- Integrations and background jobs work as expected.
- New SSL certificates are issued and validated correctly.
- You can create and restore a test backup on a non-production instance.

## 2) Migrating a Single WordPress Instance to Another Hosting Server

This section applies when PanelAlpha stays on the same machine, but you want to move one WordPress website from hosting server **A** to hosting server **B** (for example new hardware, new control panel, different region).

### Prerequisites

- The target hosting server is added in PanelAlpha: [Hosting Servers](hosting-servers.md)
- You have a plan/service ready on the target server so PanelAlpha can provision the destination.
- You have a migration window (DNS and SSL changes can require propagation time).

### Option A (Recommended): Client Area Import (Automatic Migration)

Use PanelAlpha's Client Area flow to import WordPress from the source server into a newly created instance on the target server. This creates an **automatic migration** that admins can monitor in the Admin Area.

1. Create a new instance on the target server and choose **Import Existing WordPress Instance**: [Importing Existing Instance](../client-area-instances/importing-existing-instance.md)
2. Provide access to the source website (typically FTP/SFTP credentials).
3. Monitor the import in the Admin Area (progress, errors, retries): [Migrations](../admin-area/migrations.md)
4. After the import completes, verify:
   - Frontend pages and assets
   - WP Admin login
   - Plugins that rely on cron or scheduled actions
   - Outgoing email (if your setup sends mail through the server)
5. Switch traffic to the new instance (DNS / domain move).

If you are also changing the domain in PanelAlpha, follow: [Changing Domain](../client-area-instances/changing-domain.md).

### Option B: Admin Area Migration (Manual Migration)

Choose this option when:

- The Client Area import failed and you want an admin to continue the migration.
- You want the admin team to handle the file and database upload steps.
- You need more control over the process (verify files before continuing, review logs, retry, or mark successful).

Recommended flow:

1. In the Admin Area, open the migration center: [Migrations](../admin-area/migrations.md)
2. Click **Manual Migration**.
3. Select the **User** and the **Service** (this determines where the destination instance will be created), then click **Create Instance and Initiate Import**.
4. Upload the required files and database using the provided FTP credentials shown in the migration details.
5. Click **Verify**, then **Continue Import** to finish.
6. Verify the website on the destination instance.
7. Switch traffic (DNS / domain move).

For the full step-by-step manual procedure, follow: [Migrations – Manual migration](../admin-area/migrations.md#manual-migration).

## 3) Migrating All Instances from One Hosting Server to Another

When migrating a whole hosting server, the safest approach is to treat it as a batch of **single-instance migrations**.

Recommended process:

1. Add and validate the destination hosting server in PanelAlpha.
2. Migrate instances one by one using Option A (import) or Option B (manual).
3. Monitor and manage failures centrally: [Migrations](../admin-area/migrations.md)
4. After validation, move DNS for each domain and decommission the old server.
