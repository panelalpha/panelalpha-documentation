---
title: DirectAdmin
description: Troubleshoot PanelAlpha DirectAdmin issues including queue task failures, subdomain ownership, MySQL host configuration, and account isolation.
nav:
  group: Server Issues
  order: 4
---


# DirectAdmin

This page covers common DirectAdmin-related issues in PanelAlpha.

### Issues with Queue Tasks

**Problem:** Task failures related to synchronizing WordPress data, refreshing data, and staging tasks. Common log entries in **Admin Area → Logs → Tasks** include:

- `Cannot connect to API. Server is not configured properly.` — the DirectAdmin connector has no API URL configured for the server.
- `Unauthorized` — HTTP 401; the DirectAdmin login credentials stored in PanelAlpha are wrong or expired.
- `License is overused` — HTTP 402; the DirectAdmin license has exceeded its account limit.
- `Access denied` — HTTP 403; the configured account lacks permission for the requested action.
- `Connection Error: <error>` / `Connection Failed` — a cURL-level failure reached PanelAlpha (for example cURL error 28, connection timeout). The incident is labeled `Connection Timeout` or `Connection Failed`.
- `Domain not found` — the domain referenced by the task does not exist on the DirectAdmin account.
- `Cannot delete main domain` — the task attempted to remove the account's main domain, which DirectAdmin forbids.
- `Cannot generate valid mysql database name` — the generated database name collided with the DirectAdmin username prefix.
- `SSO Login to phpMyAdmin is disabled. Please contact with server administrator.` — SSO into phpMyAdmin is disabled on the DirectAdmin server.
- `PHP version <version> not found on server.` — the requested PHP version is not installed/available on the DirectAdmin server.
- `API Access Denied` — incident label used when the DirectAdmin API rejects the request with HTTP 403.

**Cause:** The associated domain or account was deleted in DirectAdmin but the instance remains in PanelAlpha, the stored credentials are no longer valid, or the DirectAdmin server is unreachable / rejecting API calls.

**Solution:** Verify that the account and domain are properly configured in DirectAdmin and match the instance in PanelAlpha. Confirm the DirectAdmin API URL and login credentials stored in PanelAlpha are still valid, the DirectAdmin license is not exceeded, the requested PHP version is available on the server, and that the PanelAlpha host can reach the DirectAdmin API (check for cURL timeouts if `Connection Error` appears).

### Subdomain Ownership Check Blocks Instance Creation

**Problem:** Instance creation fails with errors that the domain or subdomain does not belong to the user.

**Solution:** Set `check_subdomain_owner=0` in `directadmin.conf` and restart DirectAdmin to allow creating subdomains across accounts. See the DirectAdmin [documentation](https://docs.directadmin.com/changelog/version-1.33.1.html#option-to-prevent-creating-subdomain-of-a-different-user).

### Change Default MySQL Host for DirectAdmin Connector

**Problem:** You need to override the MySQL host used by the connector.

**Solution:** Run:

```bash
docker compose -f /opt/panelalpha/app/docker-compose.yml exec -T api php artisan server:set-details ID db_host VALUE
```

Replace `ID` with the PanelAlpha Server ID (for example, `1`) and `VALUE` with the desired host (for example, `localhost:/tmp/mysql.sock`).

### Isolate Instances with Separate DirectAdmin Accounts

**Problem:** Multiple instances share one DirectAdmin account, increasing risk if one site is compromised.

**Solution:** Create separate DirectAdmin accounts per instance instead of reusing the same account.
