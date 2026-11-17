---
title: "Editions and licence"
description: "PanelAlpha Single Server Edition comes in three editions: Single Server Free, Single Server Plus and Single Server Pro. All three manage the one PanelAlpha..."
nav:
  group: Editions
  order: 1
---

# Editions and licence

PanelAlpha Single Server Edition comes in three editions: Single Server Free, Single Server Plus and Single Server Pro. All three manage the one PanelAlpha Engine they run on. The edition sets how many people and plans the panel supports and whether it has an Admin Area.

## What each edition allows

**Single Server Free** is for one person on one server. It needs no licence key.

- One Engine and one account, the first one. It signs in to the Client Area, the part of the panel where you manage projects.
- No project limit.
- No Admin Area.
- No shared access to projects and no sign-up or billing pages. The project table still has a **Shares** column, and it stays empty.
- Your own pages in the user menu: **Tasks** and **Settings**: [Settings](../the-panel/settings.md).

**Single Server Plus** adds the Admin Area to the same single server: [Admin Area](admin-area.md).

- One default user, one default service and one plan.
- Unlimited projects.
- The plan's domain settings are read only.

**Single Server Pro** is for a server with several people on it.

- Many users, many services and several plans.
- Plan domain settings you can change.
- Shared access to projects, with roles and invitations: [Sharing projects](sharing-projects.md).

In the Admin Area the first licence tier is also named `Single Server Free`. It is not the same as the edition with no licence. It is a free licence key that the licence server hands out to a server address, with a limit on projects (see below), and it runs in the paid panel build with an Admin Area. The edition with no licence has no key, no Admin Area and no project limit.

## The licence

The paid editions check a licence. The panel's Admin Area shows it in the **System** card at the bottom of the **Configuration** menu: the version, when the licence expires, whether billing is paid, the **License Type** and **License For**. The licence type reads `Single Server Free`, `Single Server Plus`, `Single Server Pro` or `Multi Server`.

<!-- TODO screenshot: assets/licence-system-card.png. Show the System card at the bottom of the Configuration menu with the version, the licence expiry and the licence type. No real licence key. -->

The licence also sets a limit on projects. The panel counts all projects on the server against it:

- Under the licence type `Single Server Free` the limit is the one on the licence. A licence that names none gives 5.
- Under `Single Server Plus` projects are unlimited.
- Under `Single Server Pro` the limit comes from the plan. The Default Plan starts with projects unlimited and 10 stored as the limit.

There is no field for the licence key in the Admin Area or the setup wizard, and the panel does not read it from a setting you pass at deploy. The only way is a command inside the panel's container. Run it on the server as root, with your panel project name and your key:

```bash
docker exec -i panel docker exec -i -u www-data panelalpha-v1-panel php /app/backend/artisan system-settings:set licensekey YOUR-KEY
```

It prints `DONE`. The panel checks the key at the next Admin Area request.

The licence check is code that the release build adds. It runs on every Admin Area request except the first-run wizard, and it keeps the licence server's answer in the `localkey` and `licensedata` settings. A failed check answers the request with status 422 and the message the licence check gives. The Client Area does no licence check, so your users can still sign in and your sites keep running. The Admin Area's licence refresh clears the stored answer and asks the licence server again.

## Upgrading

The public repository builds Single Server Free only. It has none of the paid packages. Single Server Plus and Single Server Pro are one paid build (the packages `edition-pro` and `admin-area-controller`), and the licence decides between them. Multi Server is another paid build, which adds `edition-multi`. All of them are built from private repositories, so you cannot build them from the public one. To move up, deploy the paid build to the same panel project instead of a new one. The project keeps its data, so your account, settings and projects stay. At the first start the panel gives your account a service on a `Default` plan with unlimited projects and links every existing project to it. Then add your licence key as above and add the `admin.` domain: [Admin Area](admin-area.md).

## Multi Server

Multi Server is a separate product for managing many Engines from one panel. These pages do not cover it.

## When a limit stops you

The Admin Area refuses a second plan, a change to the plan's domain settings and a project over the limit with a message: [Admin Area](admin-area.md).
