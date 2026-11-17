---
title: Cloudflare
description: Fix PanelAlpha Cloudflare issues including DNS zone errors, connection failures, SSL problems, propagation verification, and API/proxy timeouts.
nav:
  group: Server Issues
  order: 1
---


# Cloudflare

This page covers common Cloudflare-related issues in PanelAlpha, including DNS, SSL, and API timeout problems.

### Problem with Creating DNS Zones

**Problem:** Cloudflare DNS zones are not being created correctly, with the error `Resource not Found`.

**Solution:**
1. Verify the API token settings to ensure proper permissions for Zone in the Cloudflare panel. Read more [here](https://developers.cloudflare.com/fundamentals/api/get-started/create-token/).
2. Check notifications in the PanelAlpha admin area for more details.

### Connection Issues with Cloudflare

**Problem:** The test connection fails with Cloudflare.

**Solution:** Check your API privileges and follow the instructions in the [Cloudflare documentation](../../multi-server/dns-servers/cloudflare.md).

### Server Name in Cloudflare Server Configuration

**Question:** What should be entered for **Server Name** under **DNS → Add Cloudflare server**?

**Answer:** The server name is for internal use — choose any name to easily identify it.

### DNS Propagation Is in Progress

**Problem:** In the Client Area you see messages like "DNS propagation is in progress," or verification keeps failing when using Cloudflare.

**Why this happens with Cloudflare:**
- If your plan uses verification mode **Compare A record with hosting IP address**, Cloudflare's proxy (orange cloud) returns Cloudflare edge IPs, not your origin IP, so the A-record check fails.
- If your plan uses **Compare NS records with DNS server's nameservers** but the domain is delegated to Cloudflare nameservers (or to a different provider than your selected DNS server), the NS check fails.
- If your plan uses **Compare CNAME record with custom domain** but the allowed targets are not configured correctly, the CNAME check fails.

**Solution:** (Align with **Plans → DNS → Advanced DNS Configuration → Client Area → Domain DNS Verification**)
1. For proxied Cloudflare zones, prefer:
   - **Compare CNAME record with custom domain** and set **Valid CNAME Addresses** to your expected targets (for example, your platform domain like `example.com` or onboarding domain like `*.apps.example.com`). Wildcards are supported (for example, `*.quicns.com`).
   - Or select **Don't verify** to disable verification and hide propagation messages.
2. Avoid **Compare A record with hosting IP address** for proxied domains. If you must use it, temporarily turn the proxy off (gray cloud) so the record resolves to the origin IP, complete verification, then re-enable the proxy.
3. Use **Compare NS records...** only if the domain is expected to use the nameservers of your selected DNS server. If the domain stays on Cloudflare (or any third-party DNS), this check will not pass — switch to CNAME or Don't verify.

**Tip:** Ensure your plan's DNS settings match your actual DNS architecture. If PanelAlpha creates zones on an internal DNS but clients point domains to Cloudflare, NS verification will fail by design — use CNAME verification or disable verification.

### SSL Order Stuck on "Awaiting DNS Propagation" or Skipped

See [Automatic SSL Issues](../configuration/automatic-ssl.md) for SSL order troubleshooting.

### Zones or Records Not Created or Cleaned Up

**Problem:** New instances do not create Cloudflare zones or records, or deleted instances leave stale records.

**Solution:**
1. Confirm the plan has the Cloudflare DNS server assigned.
2. In **Admin → Configuration → Servers → DNS Servers → Zones**, use **Import Zone** to resync.
3. If **Auto Delete DNS Zones** was disabled, remove leftover zones or records directly in Cloudflare.
4. For aliased root domains, ensure both the root and `www` are allowed; keep them unproxied during initial validation.

### DNS Zone Manually Deleted on Cloudflare — How to Reassign

See [DNS Zone Manually Deleted](../common/common-error-solutions.md#dns-zone-manually-deleted-on-dns-server) for the resolution procedure.

### Invalid Mail TXT Records

**Problem:** TXT mail records (SPF, DKIM, DMARC) show as invalid in Cloudflare.

**Solution:** Cloudflare requires TXT values to be quoted. Recreate the records with quoted values, then retry validation (for example, for WP Cloud transactional mail).

### Accessing Admin Panel Without Exposing Port 8443 Using Cloudflare Tunnel

**Problem:** The PanelAlpha admin panel requires port 8443, which may not be accessible or desired in certain network configurations.

**Solution:** Use Cloudflare Tunnel to map your admin panel URL to the required port without exposing port 8443 directly:

1. Set up a Cloudflare Tunnel for your domain.
2. Map your admin panel subdomain (for example, `admin.domain.com`) to your PanelAlpha server's port 8443 through the tunnel.
3. Configure your firewall to allow connections through Cloudflare.

**Benefits:**
- Admin panel remains secure and accessible without opening port 8443 to the public internet
- Firewall protection through Cloudflare's security features
- No impact on SSO or user login functionality
- PanelAlpha validates only the domain suffix (for example, `*.domain.com`), so the tunnel mapping works seamlessly

**Note:** This approach does not affect SSO authentication or client-area login procedures, as PanelAlpha only validates that the domain belongs to your configured domain.

### API Requests Timing Out When Cloudflare Proxy Is Enabled

**Problem:** Requests PanelAlpha makes to the Cloudflare API (or to its own API through a Cloudflare-proxied domain) fail or time out. Because the PanelAlpha codebase does not implement a dedicated webhook subsystem, these timeouts surface as generic connection errors in the task/API logs rather than as webhook-specific messages. Common log entries include:

- `Connection Error: Connection timed out` — a cURL error 28 reached PanelAlpha; the request exceeded the configured timeout before completing.
- `Connection Timeout` — incident label assigned to the failed task.
- `Connection Failed` — incident label used when the connection could not be established at all.
- Raw `errors[].message` values from the Cloudflare API response — Cloudflare API errors are passed through verbatim, so the exact wording comes from Cloudflare's `errors` array.
- `Invalid account` — the API token is wrong, expired, or lacks the required permissions.
- `Unable to get zones` / `Unable to get DNS records` — Cloudflare rejected the request to list zones or records (often token scope or permission issues).
- `You cannot purge cache now. Check that nameservers are set up.` — a cache purge was attempted on a zone whose nameservers are not pointed at Cloudflare.
- `Invalid DNS Server` — the zone is not configured as a Cloudflare-type zone in PanelAlpha.
- `Your request could not be processed as it was blocked by an anti-bot protection system.` — Cloudflare's bot protection blocked a request (for example a WordPress login attempt routed through Cloudflare).

> [!NOTE]
> **Note:** `504 Gateway Timeout` is not emitted by PanelAlpha as a string. When Cloudflare terminates a long-running proxied request, PanelAlpha observes it as a cURL error 28 and reports it via `Connection Error` / the `Connection Timeout` incident label.

**Why this happens:**

When PanelAlpha issues an HTTP request that traverses Cloudflare's proxied (orange-cloud) path, Cloudflare enforces edge timeouts on proxied connections. Long-running operations — large cache purges, bulk DNS record operations, or requests to PanelAlpha's own API through a proxied domain — can exceed this limit, so Cloudflare closes the connection before a response is received and PanelAlpha records a `Connection Error` / `Connection Timeout`.

**Solution:**

1. **For PanelAlpha → Cloudflare API calls:** verify the API token is valid, not expired, and has the correct permissions (Zone and DNS edit scope). If you see `Invalid account` or `Unable to get zones`, regenerate the token in the Cloudflare dashboard and re-enter it in PanelAlpha under **DNS → Cloudflare server**.
2. **For requests to PanelAlpha's own API through a proxied domain:** bypass Cloudflare's proxy for internal traffic. Either point internal calls directly at the server IP, or use a separate subdomain with the Cloudflare proxy disabled (gray cloud) so the request reaches the origin without edge timeouts.
3. **For `Invalid DNS Server` / purge errors:** confirm the zone is delegated to Cloudflare nameservers and that the zone is configured as a Cloudflare-type DNS server in PanelAlpha before performing purge or record operations.
4. **For anti-bot blocks:** if legitimate requests (such as WordPress logins) are being blocked with the anti-bot message, relax the Cloudflare security level or add an exception for the affected path in the Cloudflare dashboard.
5. After applying changes, re-run the failing task and check **Admin Area → Logs → Tasks** (and the API logs via `docker compose -f /opt/panelalpha/app/docker-compose.yml logs -f api`) to confirm the timeout no longer recurs.
