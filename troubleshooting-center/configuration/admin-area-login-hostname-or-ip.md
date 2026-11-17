---
title: Admin Area Login by Hostname or IP
description: Fix admin area login when it works by IP but fails by hostname or Cloudflare Tunnel, including after snapshot restore or server migration.
nav:
  group: Configuration Issues
  order: 6
  title: Admin Login by Hostname or IP
---


# Admin Area Login by Hostname or IP

The admin area listens on port **8443** by default. Many installations expose it through a **hostname** (often via **Cloudflare Tunnel** on port **443** without `:8443` in the browser). Login can work on one address and fail on another if the server is not configured for every address you use.

This often appears **after a snapshot restore** or **server migration**, when only the old domain or the new IP was left in configuration.

| Installation | Panel directory | Env file for Sanctum |
| --- | --- | --- |
| Multi-server | `/opt/panelalpha/app` | `.env-api` |
| Single-server | `/opt/panelalpha/app-lite` | `.env` |

## Admin Login Works by IP but Not by Domain

**Problem:** Credentials work at `https://<public-ip>:8443`, but fail when you open the admin URL by **hostname** (for example through Cloudflare Tunnel). You may see a server error, a login loop (success then back to login), or no session after submit.

**Cause:** Admin login uses cookie-based sessions. Each **host and port** you use in the browser must be allowed in panel configuration. After restore or manual changes, Sanctum may list only the IP or only one domain.

Keep these rules in mind:

- **`SANCTUM_STATEFUL_DOMAINS`** must list **every** address you use: each hostname (with and without `:8443`), and the public IP (with and without `:8443`).
- Leave **`SESSION_DOMAIN` empty**. Do not set it to a raw IP.
- **Do not add the public IP to trusted hosts** (`system-settings:trusted-hosts`). Trusted hosts are for **domain names** only; an IP there can cause the browser to reject session cookies.
- If you use **more than one hostname** for the same panel (for example two tunnel names), add **each** hostname to Sanctum and trusted hosts.

> [!NOTE]
> **Note:** `docker compose logs api` shows the container supervisor only. For PHP errors during login, check `storage/logs/laravel.log` inside the `api` container while reproducing the issue.

**Solution (multi-server):**

```bash
cd /opt/panelalpha/app

# Check current values
grep -E '^(SANCTUM_STATEFUL_DOMAINS|SESSION_DOMAIN)=' .env-api
docker compose exec -T api php artisan system-settings:trusted-hosts:list
docker compose exec -T api php artisan tinker --execute="echo App\Models\Setting::get('adminapp.url');"

# Keep SESSION_DOMAIN empty
grep -q '^SESSION_DOMAIN=' .env-api && \
  sed -i 's|^SESSION_DOMAIN=.*|SESSION_DOMAIN=|' .env-api || \
  echo 'SESSION_DOMAIN=' >> .env-api

# One comma-separated line — replace hostnames and IP with yours
sed -i 's|^SANCTUM_STATEFUL_DOMAINS=.*|SANCTUM_STATEFUL_DOMAINS=admin.example.com,admin.example.com:8443,alt-admin.example.com,alt-admin.example.com:8443,203.0.113.10,203.0.113.10:8443|' .env-api

# Add each admin domain (repeat per hostname; never add the IP here)
docker compose exec -T api php artisan system-settings:trusted-hosts:add admin.example.com
docker compose exec -T api php artisan system-settings:trusted-hosts:add alt-admin.example.com

docker compose restart api queue-worker
```

Optional — set the stored admin URL to the hostname users bookmark (tunnel URL without `:8443`):

```bash
docker compose exec -T api php artisan settings:set adminapp.url "https://admin.example.com"
```

**Solution (single-server):** Use `/opt/panelalpha/app-lite`, edit **`.env`** instead of `.env-api`, then restart `api` and `queue-worker` and add trusted hosts the same way.

**After changing configuration:**

1. Clear cookies for the admin hostname or use a private/incognito window once.
2. Log in with the **exact URL** you intend to support.
3. If login still fails, capture the browser URL and run:
   ```bash
   docker compose exec -T api tail -80 storage/logs/laravel.log
   ```

## Tunnel Returns an Error but IP:8443 Works

**Problem:** The public hostname (Cloudflare Tunnel) returns **502/503** or a server error, while `https://<public-ip>:8443` works.

**Cause:** Cloudflared may not reach admin nginx on port **8443**, or the tunnel origin URL is wrong. This is separate from Sanctum configuration.

**Solution:**

```bash
systemctl status cloudflared --no-pager
journalctl -u cloudflared -n 50 --no-pager
curl -k -sS -o /dev/null -w '%{http_code}\n' -I -H 'Host: admin.example.com' https://127.0.0.1:8443/
```

Confirm the tunnel **service** points at the admin HTTPS endpoint on **8443** (for example `https://127.0.0.1:8443`), not the client area on port 443.

## Related Issues

- **Admin 2FA fails after snapshot restore** — preserve the original `APP_KEY` from the snapshot. See [Snapshot Tool Issues](../backups/snapshot-tool-issues.md).
- **Wrong domain or IP after restore** — see [Restored Instance Shows Wrong Domain/IP Addresses](../backups/snapshot-tool-issues.md#restored-instance-shows-wrong-domain-ip-addresses) and the hostname login steps above.
