---
title: Installation and Wizard
description: Fix PanelAlpha installation issues including system freezes, clock errors, port conflicts, Docker build failures, and SSL connection errors.
nav:
  group: Installation Issues
  order: 1
  title: Installation & Wizard
---


# Installation and Wizard

### PanelAlpha Installation Problems

**Problem:** PanelAlpha system installation is stuck, or the server is freezing or crashing.

**Solution:** Ensure your server has sufficient memory — a minimum of 4 GB RAM is required. Check with `free -h` and `docker compose ps`; a common cause is insufficient RAM (minimum 4 GB).

### Error During System Installation

**Problem:** Installation fails with the error:

```
CLOCK: adj_systime: Operation not permitted ntpdate ntp.ubuntu.com command failed with exit code 1
```

**Cause:** LXC containers do not allow time adjustment (`adj_systime`).

**Solution:** Ensure you are on a VPS or VM. Installation of the PanelAlpha application or engine is not supported in a container environment.

### Using PanelAlpha and WHM on One Server

**Question:** How many servers are needed to use PanelAlpha and cPanel?

**Answer:** You need two separate servers — one for PanelAlpha and one for cPanel (WHM). It is not possible to run both on a single server simultaneously.

### Connection Error During Server Configuration

**Problem:** The error `Connection Error: SSL certificate problem: self-signed certificate` appears during server configuration.

**Solution:** Make sure the server you are trying to connect to has a valid SSL certificate, or untick the **SSL Verification** checkbox to skip verification.

### Installing PanelAlpha with a WHM Reseller Account

**Question:** Can PanelAlpha be installed using a WHM reseller hosting account?

**Answer:** No. Installing PanelAlpha on WHM Reseller Hosting accounts is not possible. A root account is required.

### How to Regenerate Your PanelAlpha Engine API Token

See [Lost API Token](../servers/panelalpha-engine.md#lost-api-token-your-own-software) for the regeneration procedure.

### Port Conflicts When Updating PanelAlpha

**Problem:** During a PanelAlpha update, an error occurs when the system tries to restart containers:

```
Error response from daemon: driver failed programming external connectivity on endpoint app-nginx-1 (...):
failed to bind port 0.0.0.0:80/tcp: Error starting userland proxy: listen tcp4 0.0.0.0:80: bind: address already in use
```

**Cause:** Apache2 is running on port 80, conflicting with the `app-nginx` container.

**Solution:**
1. Stop the Apache2 service to free port 80:
   ```bash
   sudo systemctl stop apache2
   ```
2. (Optional) Prevent Apache2 from restarting on server reboot:
   ```bash
   sudo systemctl disable apache2
   ```

This ensures Apache2 does not block port 80 after future reboots, allowing PanelAlpha containers to start without issue.

### Docker Build Errors During PanelAlpha Updates

**Problem:** The following error appears during PanelAlpha updates:

```
target api: failed to solve: process "/bin/sh -c docker-php-ext-install opcache pdo pdo_mysql zip pcntl" did not complete successfully: exit code: 2

docker compose -f /opt/panelalpha/app/docker-compose.yml build api websocket queue-worker cron command failed with exit code 1
```

**Cause:** Server-side security or monitoring software is killing Docker processes during the build. Examples include:
- **BitNinja Process Analysis**
- Other security or monitoring software that may interfere with Docker container builds

**Solution:** Check if any security or monitoring software on your server might be terminating Docker processes during the build phase. Contact your server administrator or hosting provider to whitelist Docker processes or temporarily disable such software during PanelAlpha updates.
