---
title: Looking after your project
description: "Once a project is online, you keep using the same chat. Add a domain, take a backup, make a test copy or ask why a page looks wrong."
nav:
  group: Getting Started
  order: 5
---

# Looking after your project

Once a project is online, you keep using the same chat. Add a domain, take a backup, make a test copy or ask why a page looks wrong. You do not need a control panel for this.

```text
List the projects on this engine.
```

What this does: the assistant shows every project on this VPS.

What you should see: names and addresses. Older versions called a project a "user". `username` in the output is still the project's name.

## Add your own domain

Point the DNS record at your VPS first, then ask:

```text
Add shop.example.com to this project and request a Let's Encrypt certificate for it.
```

Name a domain when you create a project only if it already points here. If you leave it out, the engine picks a public name and tells you which. Full page: [Domains and HTTPS](../capabilities/domains-and-ssl.md).

To reach a project through Cloudflare instead of pointing DNS straight at the VPS: [Cloudflare](../capabilities/cloudflare.md).

## Take a backup, and restore if it goes wrong

```text
Create a backup of this project.
```

A restore overwrites the live project, so list backups first if you are unsure. Full page: [Backups](../capabilities/backups.md).

## Make a test copy

```text
Make a staging copy of this project.
```

You get a second project with its own hostname. The hostname is `staging.` in front of the live one's. For a project on a free `panelalpha.online` name, it is a new name of its own there. You can break this copy freely. When you are happy with it:

```text
Push the staging copy live.
```

You can also push live to staging, to refresh the test copy with real data. A project can have one staging copy. Full page: [Projects](../capabilities/projects.md#staging).

## Rebuild after you change files

```text
Rebuild this project.
```

Use this after you push new commits, or change files over FTP on a project that has no repository. A rebuild replays the last successful plan: [How a deploy works](how-a-deploy-works.md).

**A rebuild of a project connected to git clones its branch again, so files changed on the server are lost.** Commit them to the repository instead.

An environment variable does not need a rebuild. See below.

## Change an environment variable

On a project that runs in its own container, the assistant can show and change its environment variables without a rebuild. Listed values are hidden.

```text
Show the environment variables of this project, then set APP_MODE to staging.
```

The application restarts in place with the new value. A value that the build itself reads, as Vite and Next.js do, still needs a rebuild. A removed variable is gone at once, but a default that the project type sets for it comes back at the next deploy. Some names belong to the engine (the ones starting `PANELALPHA_ENGINE_`), and you cannot set them. A project on traditional PHP hosting does not have this feature. Details: [Environment variables](../capabilities/projects.md#environment-variables).

## See what was deployed before

```text
List the recent deploys of this project.
```

By default the engine keeps the last 10: [How a deploy works](how-a-deploy-works.md#past-deploys).

## See why a page is broken

```text
This project does not open properly. Read the deploy log, check what it is
actually serving, and tell me what is wrong.
```

The engine looks from inside the account and names what it finds: [Monitoring and logs](../capabilities/monitoring-and-logs.md#what-the-engine-checks).

## Build caches

A deploy keeps copies of downloaded packages, so the next deploy of that project does not fetch them again. Once a day the engine deletes those copies for any project that has not deployed in the last 24 hours. A project that deploys every day keeps them. A deploy that is still running is left alone. The next deploy after a cleanup downloads the packages again. You do not have to ask for this cleanup. To run it yourself: [CLI commands](../commands/pae-cli.md#advanced-and-server-maintenance).

## Other day-to-day work

- **WordPress.** Ask for a new site without your own code, or look after WordPress that is already on this engine: [WordPress](../capabilities/wordpress-and-apps.md).
- **A database, FTP or SFTP, or a scheduled task** that the application needs: [Databases](../capabilities/databases.md) · [Files, FTP and SFTP](../capabilities/files-and-access.md) · [Projects](../capabilities/projects.md#cron-jobs).
- **Git.** Pull the latest commit, switch branch or reconnect a private repository: [Connecting with Git](../capabilities/connecting-with-git.md).

More examples: [What to ask](../connecting-your-ai/your-assistant.md#what-to-ask).
