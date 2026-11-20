---
title: PanelAlpha Engine
description: "Resolve PanelAlpha Engine 2.0+ issues: lost tokens, install and update failures, assistant connection, containers, performance, domains, and vhost repair."
nav:
  group: Server Issues
  order: 2
---


# PanelAlpha Engine

This page is for **PanelAlpha Engine 2.0+** on a standalone VPS (install and update with `get.panelalpha.com/engine`, projects driven from an AI assistant or `pae`). PanelAlpha Single Server Edition works with **Engine 2.3.0 and later**. PanelAlpha Multi Server Edition still uses **Engine 1.x**. For that stack, use the [Engine v1 documentation](../../panelalpha-engine/v1/index.md).

The full operator guide lives under [PanelAlpha Engine documentation](../../panelalpha-engine/index.md).

### Lost assistant (MCP) token

**Problem:** Your AI assistant can no longer connect, or you never saved the token from install.

**Solution:** On the VPS as `root`, mint a new token and copy the command for your assistant:

```bash
pae connect
```

Or name the assistant, for example:

```bash
pae connect claude
```

To list or revoke tokens:

```bash
pae mcp:token:list
pae mcp:token:revoke <id>
```

Check that HTTPS and the token work on the server:

```bash
pae mcp:check <token>
```

Details: [Create a token](../../panelalpha-engine/connecting-your-ai/create-a-token.md).

### Lost API token (your own software)

**Problem:** You need a token for a script or integration, not for an assistant.

**Solution:** Assistants use MCP tokens from `pae connect`. API integrations use a different kind:

```bash
pae api:token:create my-integration
```

See [CLI commands](../../panelalpha-engine/commands/pae-cli.md).

### Installation or update fails

**Problem:** `curl -fsSL https://get.panelalpha.com/engine | sh` stops with an error, or the engine version did not change after an update.

**Solution:** Engine 2.0+ does not ask for a license key at install. Common messages:

- `Run as root` / `Please run as root!` — log in as `root` over SSH, not `sudo` only.
- Unsupported operating system — use Debian 12 or 13, or Ubuntu 22.04, 24.04, or 26.04 ([Install](../../panelalpha-engine/getting-started/install.md)).
- Port already in use — free 80, 443, 2011, 21, 30000–30009, and 2222; also 53 if the installer needs local DNS.
- Install failed partway — do not run the curl line again on top. [Uninstall](../../panelalpha-engine/getting-started/uninstall.md), reboot the VPS, then install again.
- `Could not obtain a download token` / `Invalid download status` during update — the engine package was not replaced. Contact [support](https://manage.panelalpha.com/contact).
- Version unchanged after update — see [Updating](../../panelalpha-engine/getting-started/updating.md).

To save install output:

```bash
curl -fsSL https://get.panelalpha.com/engine | sh 2>&1 | tee install.log
```

Update runs also log under `/opt/panelalpha/log/engine-updates/`.

### Cannot connect your AI assistant

**Problem:** The assistant reports certificate errors, 401, or cannot reach the engine.

**Solution:**

- Run `pae connect` on the **VPS**, then paste the printed command on **your computer**.
- Use an MCP token from `pae connect` or `pae mcp:token:create`, not `pae api:token:create`.
- Your computer must reach the engine on port **2011** (firewall, correct `APP_URL`, certificate trusted on the client). Start from [Connecting your AI](../../panelalpha-engine/connecting-your-ai/your-assistant.md).

### Cannot connect PanelAlpha Single Server or Multi Server

**Problem:** You expect the PanelAlpha control panel to manage this host.

**Solution:** Single Server Edition needs Engine **2.3.0 or later**. On an older engine, update it first: [Updating](../../panelalpha-engine/getting-started/updating.md). Multi Server Edition does not support Engine 2.x yet. It uses Engine 1.x: [Engine v1 documentation](../../panelalpha-engine/v1/index.md).

### Docker containers not starting

**Problem:** Engine containers fail to start or stay unhealthy.

**Solution:**

- Confirm Docker is running on the host.
- Inspect the stack:

```bash
docker compose -f /opt/panelalpha/shared-hosting/docker-compose.yml ps
docker compose -f /opt/panelalpha/shared-hosting/docker-compose.yml logs core --tail 200
```

- Check disk space (`df -h`). A full disk stops containers and deploys.

### Performance issues

**Problem:** The VPS or projects feel slow; CPU or RAM stays very high.

**Solution:**

- Run `docker stats` and `pae system:version` on the host.
- Look for one project using most resources; check its usage in chat or via monitoring docs in [Capabilities](../../panelalpha-engine/capabilities/monitoring-and-logs.md).
- If the whole VPS is undersized, add RAM or CPU at your provider.

### Missing `www` on a project domain

**Problem:** `example.com` works but `www.example.com` does not.

**Solution:** On the VPS as `root`:

```bash
pae project:domain:add-www-alias --all
```

For one project:

```bash
pae project:domain:add-www-alias --project=<project>
```

Ensure DNS for `www` points at the VPS, then wait for propagation.

### WordPress and WP-CLI

**Problem:** You need WP-CLI or WordPress maintenance on a site on this engine.

**Solution:** Prefer your connected assistant, for example:

```text
List plugins on the WordPress project named shop.
```

How WordPress is hosted on Engine 2.0+ (container vs traditional `public_html`): [WordPress and known apps](../../panelalpha-engine/capabilities/wordpress-and-apps.md).

### Orphaned site configs after a manual delete

**Problem:** Sites stop serving or nginx-proxy will not reload after someone removed a project outside the normal flow.

**Solution:** Regenerate vhost configuration for projects that still exist:

```bash
pae project:rebuild --all --wipe-vhosts-dir
```

The public webserver on a default install is **nginx-proxy only**, not Apache or LiteSpeed on the VPS.

### General troubleshooting

**Problem:** None of the above fits.

**Solution:** Collect this before contacting [support](https://manage.panelalpha.com/contact):

1. `pae system:version`
2. `docker compose -f /opt/panelalpha/shared-hosting/docker-compose.yml ps`
3. `docker compose -f /opt/panelalpha/shared-hosting/docker-compose.yml logs core --tail 200`
4. The exact error text from install, update, or your assistant

For install, update, tokens, and day-to-day hosting, use the [PanelAlpha Engine documentation](../../panelalpha-engine/index.md).
