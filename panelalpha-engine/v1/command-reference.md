---
title: Artisan Commands
description: The Artisan commands of PanelAlpha Engine 1.x for tokens, repairs and rebuilds, with how to run them and examples.
nav:
  group: Command Reference
  order: 1
---

# Artisan Commands

PanelAlpha Engine 1.x runs on Laravel, and its maintenance commands are **Artisan commands** in the `core` container. Use them for repairs and for tasks the panel does not offer.

## Running Artisan commands

Run the commands as root on the Engine server, not inside a customer container:

```bash
docker compose -f /opt/panelalpha/shared-hosting/docker-compose.yml exec -T core php artisan <command>
```

- Replace `<command>` with a command from this page.
- `-T` turns off the interactive terminal, so the command also works in scripts.
- Most commands for hosting accounts take `--username=<username>` for one account or `--all` for every account. The username is the system user of the hosting account.
- Add `--help` to any command to see all its options.

The `core` container must be running. Check it with `docker compose -f /opt/panelalpha/shared-hosting/docker-compose.yml ps`.

## Command reference

| Command | What it does | Changes the server |
| --- | --- | --- |
| `api-tokens:create <name> [--short]` | Creates an API token for PanelAlpha. | Yes |
| `api-tokens:list` | Lists API tokens with their last use. | No |
| `api-tokens:delete <id>` | Deletes an API token. | Yes |
| `system:version` | Prints the installed Engine version. | No |
| `domains:list` | Lists all domains and aliases on the server. | No |
| `domains:wp-cli <domain> <args>` | Runs WP-CLI for the site on a domain. | Depends on the WP-CLI command |
| `users:get-limits` | Shows resource limits of hosting accounts. | No |
| `users:set-limits` | Changes resource limits of hosting accounts. | Yes |
| `users:rebuild [--wipe-vhosts-dir]` | Rebuilds the containers and domain configuration of accounts. | Yes, sites restart |
| `users:rebuild-domains` | Rebuilds the domain configuration of accounts. | Yes |
| `users:fix-domains` | Creates missing document root folders. | Yes |
| `users:add-missing-www-domain-aliases` | Adds missing `www.` aliases to domains. | Yes |
| `users:fix-file-permissions` | Resets owner and permissions of account files. | Yes |
| `users:rebuild-quotas` | Applies disk quotas again. | Yes |
| `users:cleanup-disconnected-domains` | Removes configuration of domains that no longer exist. | Yes |
| `users:delete <username>` | Deletes accounts with their files. | Yes, cannot be undone |
| `system:rebuild-domains` | Rebuilds the configuration of all domains. | Yes |
| `system:cleanup-disconnected-domains` | Removes configuration of domains that no longer exist, for all accounts. | Yes |
| `system:rebuild-modsecurity-config` | Writes the ModSecurity configuration again. | Yes |
| `system:rebuild-exim-config` | Writes the outgoing mail configuration again. | Yes |
| `system:rebuild-sftp-accounts` | Writes the SFTP accounts again. | Yes |
| `system:sync-ips-to-interface` | Adds the IPs from IP Management to the network interface. | Yes, network |
| `system:build-ipv4-nat-map` | Finds public IPs for servers behind 1:1 NAT. | Yes |
| `system:lscache --enable\|--disable` | Turns LiteSpeed Cache on or off. | Yes |
| `apache:enable-mod <mod>` / `apache:disable-mod <mod>` | Turns an Apache module on or off for accounts. | Yes |

## API tokens

PanelAlpha connects to the Engine with an API token. The installer creates the first one.

To create a token, run the command below. With `--short`, it prints only the token:

```bash
api-tokens:create <name> [--short]
```

To list tokens, or to delete one you no longer use:

```bash
api-tokens:list
api-tokens:delete <id>
```

After you create a new token, enter it in PanelAlpha in the server settings. **Deleting a token that PanelAlpha still uses cuts the connection.**

## System

### Check Engine version

```bash
system:version
```

### Sync IPs to network interface

This command adds the IP addresses from [IP Management](server-management/ip-management.md) to the server's network interface. Add `--skip-errors` to continue when one address fails. Add `--restart-networking` to restart networking afterwards.

> [!WARNING]
> **This command changes the server's network settings and can cut your SSH session.** Have console access from your hosting provider ready.

```bash
system:sync-ips-to-interface [--skip-errors] [--restart-networking]
```

### Servers behind NAT

Use this command on servers that have only private addresses and get public IPs through 1:1 NAT. It looks up the public IPv4 address for each local address and saves the mapping. `--replace-default-ipv4` also replaces the server's default IPv4 with the public address.

```bash
system:build-ipv4-nat-map [--replace-default-ipv4]
```

### Rebuild configuration

These commands write part of the configuration again from the Engine database. Use them when someone changed a file by hand or a change from the panel did not apply.

```bash
system:rebuild-modsecurity-config
system:rebuild-exim-config
system:rebuild-sftp-accounts
system:rebuild-domains
```

## Hosting accounts and domains

### Rebuild accounts

This rebuilds the containers and the web server and PHP-FPM configuration of hosting accounts. Sites of the rebuilt accounts restart.

```bash
users:rebuild --username=<username>
users:rebuild --all --wipe-vhosts-dir -v
```

> [!WARNING]
> **`--wipe-vhosts-dir` deletes all generated virtual host files before the rebuild.** Changes you made to those files by hand are lost, and sites are down until the rebuild ends.

### Add missing www aliases

Use this when `example.com` works but `www.example.com` does not.

```bash
users:add-missing-www-domain-aliases --all
users:add-missing-www-domain-aliases --username=<username>
```

### Fix file permissions

This resets the owner and permissions of files in account home folders. Use it after you copied files in as root or after a migration left wrong owners.

```bash
users:fix-file-permissions --all
users:fix-file-permissions --username=<username>
```

### Resource limits

Show the limits of one account or all accounts:

```bash
users:get-limits --username=<username>
```

PanelAlpha sets the limits from the hosting plan. Change the plan in PanelAlpha where you can. To change a limit directly, run `users:set-limits` with one of these options: `--disk-space-limit`, `--memory-limit`, `--cpu-limit`, `--device-read-bps`, `--device-write-bps`, `--bandwidth-limit`, `--mysql-databases-limit`, `--ftp-accounts-limit`, `--sftp-accounts-limit`, `--addon-domains-limit`, `--subdomains-limit`, `--inodes-limit`.

### Run WP-CLI

This runs WP-CLI as the account's user in the site's document root:

```bash
domains:wp-cli example.com plugin list
```

### Clean up removed domains

This removes web server configuration left behind for domains that are no longer in the Engine database:

```bash
system:cleanup-disconnected-domains
```

### Delete an account

**This deletes the account from the Engine database, with its configuration and home folder. It cannot be undone.** Use it only for accounts PanelAlpha no longer knows about. Normally you terminate the service in PanelAlpha.

```bash
users:delete <username>
```

## Examples

Create a token named `panelalpha-prod` and print only the token:

```bash
docker compose -f /opt/panelalpha/shared-hosting/docker-compose.yml exec -T core php artisan api-tokens:create panelalpha-prod --short
```

The output is a single line like `1|abcdef0123456789...`.

Fix file permissions for every account:

```bash
docker compose -f /opt/panelalpha/shared-hosting/docker-compose.yml exec -T core php artisan users:fix-file-permissions --all
```
