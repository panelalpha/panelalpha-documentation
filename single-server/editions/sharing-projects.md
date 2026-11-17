---
title: "Sharing projects"
description: "Sharing gives another person access to one of your projects, with a role that sets what they may do. It is in Single Server Pro only. Only the project..."
nav:
  group: Editions
  order: 3
---

# Sharing projects

Sharing gives another person access to one of your projects, with a role that sets what they may do. It is in **Single Server Pro** only. Only the project owner, or a person whose role allows managing shared access, sees **Share**.

## Roles

- **View info**: sees the project's details, logs, reports and preview. No control and no WP Admin.
- **View and SSO**: the same, and can log in to WordPress as an administrator with **WP Admin**.
- **Developer**: all management options for the site, such as staging, push, Git, redeploy, backups, plugins, themes and users. No hosting tools and no sharing.
- **Full access**: everything a Developer has, and the hosting tools (domains, SSL, FTP, SFTP, databases, cron, file manager, PHP and containers) and sharing.
- **Custom**: you pick the privileges one by one.

**You cannot grant a privilege you do not have.** The panel refuses with `Insufficient privileges`.

## Sharing a project

1. Open the project menu and select **Share**. The dialog is called `Share <project>`.
2. Select **Invite User** and enter the person's email address.
3. Choose a role in **Select Role**. For **Custom**, tick the privileges.
4. Select **Send Invite**.

   <!-- TODO screenshot: assets/sharing-invite-dialog.png. Show the Share dialog with Invite User, an example email (you@example.com), the Select Role list open (View info, View and SSO, Developer, Full access, Custom) and the Send Invite button. -->

You cannot invite yourself (`Cannot invite yourself`). A staging copy follows the shares of its live project.

The person is listed under **People with access** with the status `Pending Invitation`. **A pending invitation gives no access.** Access starts when they accept.

<!-- TODO screenshot: assets/sharing-people-with-access.png. Show People with access with one person (you@example.com) with the status Pending Invitation, plus the Resend Invite and Remove Access actions. -->

## What the invited person does

1. Open the link in the email.
2. If they have no account, enter a password under `Set Your Password` and select **Register**.

The invitation expires after 7 days. The Admin Area setting **Invitations Expiry Days** changes this. A link that has expired, been used or been cancelled shows `Invalid Invitation`.

## Changing and removing access

- Change a role in **People with access** and select **(pending changes) Save**.
- Select **Resend Invite** to send the email again. You can resend once a minute (**Invitations Resend Limit Minutes**, default 1). Sooner than that you see `You can resend the invitation again <time>`.
- Select **Remove Access** to take the person off the project.

**Invitations are emails, so mail must be set up.** The Admin Area shows `Email notifications are disabled.` with **Configure Now** when it is not.
