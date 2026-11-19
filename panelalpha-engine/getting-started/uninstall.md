---
title: Removing the engine
description: "This removes the engine from your VPS. It also permanently deletes every project on it: all files, databases and websites."
nav:
  group: Getting Started
  order: 3
  title: Uninstall
---

# Removing the engine

This removes the engine from your VPS. **It also permanently deletes every project on it**: all files, databases and websites.

There is no undo. Back up anything you want to keep first: [Backups](../capabilities/backups.md).

## Remove everything

```bash
bash /opt/panelalpha/shared-hosting/uninstall.sh
```

The script asks you to confirm before it does anything. Add `--yes` to skip the question. When it finishes, the engine and the `pae` command are gone.

Projects go first, through the engine itself. Each one loses its containers, volumes and files, as a deleted project does. Only then does anything that would stop the engine run, whatever else you asked for on the command line.

The script also removes the two services the installer added for the project homes: `panelalpha-quota` (disk quota) and `panelalpha-home-mounts` (the `nosuid,nodev` mount of `/home`). They are gone from the next boot on. Until you restart the VPS, quota and the mount stay as they are. This is one more reason to restart before you reinstall. The installer does not edit `/etc/fstab`. A quota option that an older version added there stays.

It also removes:

- The files the installer left in `/opt/panelalpha`: its own script. When no panel is installed on the VPS, it also removes the wrapper that downloaded the installer, the install id and the lock file. The panel's installer reads those too.
- The engine's logs, the directories it uses while it unpacks archives and restores backups (`/home/.panelalpha-*-stage`), and its build caches in `/var/cache`.
- The three Docker networks it made.
- The Docker Hub mirror entry it added to `/etc/docker/daemon.json`, and only that entry. A running Docker keeps using the entry until it restarts. The restart below does that for you.

Two more things are removed only when you ask, because both go beyond the engine:

```bash
bash /opt/panelalpha/shared-hosting/uninstall.sh --remove-sysbox --purge-images
```

`--remove-sysbox` removes the container runtime the engine starts projects with. It also removes the containers that run on that runtime, its service user and the `daemon.json` settings only it needs. Containers on any other runtime are left alone.

`--purge-images` removes the Docker images of the engine: the ones its stack runs, the base images it builds for projects and the ones it pulls for itself. Without that flag they stay. The uninstaller lists them so you can remove them yourself.

## What stays on the VPS afterwards

The uninstaller leaves a few things behind on purpose:

- **Your certificates.** Let's Encrypt limits how many you can request per week. If you reinstall, you keep the working ones.
- **Docker and the firewall.** The installer added them, but other things on the VPS may now depend on them. To clean the machine completely, remove them yourself or pass `--remove-docker` and `--remove-firewall`. The firewall flag turns `ufw` off and drops the engine's fail2ban jail. `--restore-systemd-resolved` puts back the machine's own handling of domain name lookups.
- **A panel** installed on the same VPS (`app` or `app-lite`). The uninstaller does not touch it. It warns that removing the engine breaks a panel that depends on this host.
- **Project files**, but only if something went wrong partway through. After a clean run they are gone.
- **Docker images** and other images from elsewhere, unless you pass `--purge-images`. The uninstaller never removes anything that the engine did not install.
- **Backups**, if there are any in `/opt/panelalpha/backups`: the engine's own update backups and the projects' backups in the default local store. The uninstaller lists them. Delete them yourself when you no longer need them. Empty store directories are removed.

## Troubleshooting

**I am moving the data elsewhere and want to keep the projects.**
This option removes the engine software but leaves the project accounts and their files on the disk. You are left with website files on a VPS where nothing runs or manages them. Use it as a step in a move, not to pause the engine. A later install gives the same user IDs to new projects, so remove the old accounts by hand before you install again.

```bash
bash /opt/panelalpha/shared-hosting/uninstall.sh --keep-projects
```

**"No such file or directory."**
The engine is not installed, or an earlier uninstall already removed it. Check with `ls /opt/panelalpha`.

**I want to reinstall from scratch.**
Uninstall, **restart the VPS**, then install again. The restart matters. The installer changes how the machine handles web addresses and network access. Restarting clears out anything left over, including the Docker Hub mirror entry that the uninstaller removed from `daemon.json`.

**Something is still running afterwards.**
The engine could not remove some projects. Its message says whether it fell back to deleting an account by hand. It lists the containers, volumes and home directories it left. Ask PanelAlpha before you delete anything by hand, especially if the VPS runs anything else.
