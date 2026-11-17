---
title: "Admin Area"
description: "The Admin Area is where you manage the panel's users, services and plans, and see the licence. It exists only in Single Server Plus and Single Server Pro."
nav:
  group: Editions
  order: 2
---

# Admin Area

The Admin Area is where you manage the panel's users, services and plans, and see the licence. It exists only in **Single Server Plus** and **Single Server Pro**.

It answers on its own address, `https://admin.<panel address>`. For a panel at `panel.example.com` that is `https://admin.panel.example.com`. A panel reached by IP address has no Admin Area address.

## Opening it for the first time

**The panel does not answer on the admin address until you add that domain to the panel project on the Engine.** Do this on the server as the operator.

1. Point the DNS of `admin.panel.example.com` at the server.
2. Add `admin.panel.example.com` as a domain of the panel project on the Engine. `panel` is the project name you gave at install. The panel's web server listens on port 80 and shows the Admin Area for every host name that starts with `admin.`. Port 8080 inside the container is the websocket server behind `/ws`, so do not point the domain at it:

   ```bash
   pae domain:create admin.panel.example.com --project=panel --proxy-to=panel:80 --force
   ```
3. Open `https://admin.panel.example.com`. If no admin exists yet, you are asked to create the first admin account.

   <!-- TODO screenshot: assets/admin-area-first-admin-form.png. Show the screen that asks for the first admin account, with example values (you@example.com). Hide the password characters. No real customer data or passwords. -->

Admins are separate from panel users. An admin has its own sign-in and password.

## Switching between the Admin Area and the Client Area

The Client Area is where you manage projects.

- In the Admin Area, select **Back to Client Area** (**Client** on a narrow screen). It opens the Client Area in a new tab, signed in as the Default User.
- In the Client Area, the **Go to Admin Area** menu takes you back. It shows **Migrations**, **Logs**, **Tasks** and **Configuration** as shortcuts. A red dot means a task is running or an import is unfinished.

<!-- TODO screenshot: assets/admin-area-go-to-menu.png. Show the Client Area top bar with the Go to Admin Area menu open and its shortcuts Migrations, Logs, Tasks and Configuration. -->

The **Go to Admin Area** link has an address only after you came from the Admin Area. Every user of a Plus or Pro panel sees the button, not only the Default User. For anyone who did not come from the Admin Area, the menu opens but its entries lead nowhere. They cannot sign in to the Admin Area through it.

## Users

Open **Users**. The tabs are **All**, **Active**, **Shared** and **Archived**.

- **Add** opens **Add New User**: **First Name**, **Last Name**, **Email**, **Password** and the boxes **Send user an email with details** and **Require user to change password**. The password follows the panel rules (at least 8 characters, upper and lower case, numbers and symbols). **Add** is hidden in Plus.
- Edit a user to change the name, company, email or password, to require a password change or to disable two-factor authentication.
- **Delete User** moves the user to **Archived**, with their services. **Nothing changes on the server.** The projects stay on the Engine. In **Archived** you can select **Restore as Active** or **Remove Permanently**.

<!-- TODO screenshot: assets/admin-area-users-list.png. Show Users with the tabs All, Active, Shared and Archived, the Add button and a few example users (you@example.com). -->

The Default User and the Default plan cannot be deleted.

## Services

A service links a user to a plan and holds their projects. Pro can have several. Plus has one.

In Pro, **Add Service** asks for **User** and **Plan**, and some screens also show **Username**, **Password** and **Domain**. The panel needs only the user and the plan. It ignores username, password and domain, so a service takes its projects from the Engine, not from those fields.

A service can also be suspended, unsuspended, moved to another owner (**Change Owner**) or deleted. Deleting is refused with `Cannot delete service with existing projects.` while it has projects.

## Plans

A plan sets what a service may do, such as the project limit and which kinds of project it allows. In Plus and Pro the menu shows one page, **Configuration > Default Plan**. It starts with projects unlimited, 10 stored as the limit, and allows WordPress, Git and Files projects and migrations. Saving a plan applies its limits to the Engine.

<!-- TODO screenshot: assets/admin-area-default-plan.png. Show Configuration, Default Plan with the project limit field and its tip about unlimited projects. -->

Pro can have more than one plan. The **Plans** menu entry is hidden, but a plan can be added with Quick Add.

## Quick Add

**Quick Add...** in the header adds things in one step. It is in Pro only. Its items are **New User**, **Service**, **Plan** and **Admin**, and **Customize Shortcut...** changes them.

## What each edition limits

- **Single Server Plus**: projects are unlimited. The project limit field is fixed and the tip says `With your current license, projects are unlimited and this setting cannot be changed. Upgrade to Pro to set a custom limit.` One plan, one service, and the domain settings of the plan are read only.
- **Single Server Pro**: several users, services and plans. The limit on projects is yours to set, within what the licence allows.

The refusals you may see:

**"Your license allows only one plan. Upgrade to Pro to create more plans."** You tried to add a second plan in Plus. Upgrade to Pro.

**"Domains settings cannot be changed with your current license. Upgrade to Pro to edit them."** The plan's domain settings are read only in Plus.

**"Application limit exceeded."** The licence or the plan allows no more projects. Delete a project you no longer need, or ask for a higher limit.

Where to read the licence, and what each tier allows: [Editions and licence](editions-and-licence.md).

## The server page

Under **Configuration > Server** the Admin Area shows the Engine, with the tabs **Details**, **IP Management**, **Firewall (CSF)**, **WAF/ModSecurity**, **Outgoing Mail**, **Notifications** and **Port Routing**. The firewall tab manages the Engine's own firewall, although it says CSF. The tabs are off during an Engine update with `Not available during an upgrade of PanelAlpha Engine`.

<!-- TODO screenshot: assets/admin-area-server-page.png. Show Configuration, Server with the tabs Details, IP Management, Firewall (CSF), WAF/ModSecurity, Outgoing Mail and Notifications, and the base domain of new projects. -->

You cannot add, edit or delete the server here: `The PanelAlpha Engine is managed by the installer and cannot be changed here.`

The **Details** tab shows this domain and lets you change it. It sets the domain that new projects are named under, as `<project>.<domain>`. Existing projects keep their domain. An empty value clears it, and a domain that cannot hold a public certificate is refused. It is the same Engine setting as **Default domain** under **Settings > General** in Single Server Free: [Settings](../the-panel/settings.md).

## Screens the Engine does not support

Some pages exist in the Admin Area but refuse the action on a PanelAlpha Engine. The message starts with `Not available on PanelAlpha Engine`. This covers:

- Relinking, reinstalling or creating a hosting account by hand.
- Changing a project's site type.
- The WP-CLI connector and the control-panel upgrade.
- Project templates, and installing custom themes or plugins.
- The WordPress migrator plugin download and manual imports.

The pages for DNS and email servers also have nothing to show, because the Engine has none.
