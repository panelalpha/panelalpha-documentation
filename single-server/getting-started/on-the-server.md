---
title: "On the server"
description: "Commands you run on your VPS as root over SSH, for the few things the panel cannot do itself."
nav:
  group: Getting Started
  order: 7
---

# On the server

These are commands you run on your VPS as `root` over SSH, for the few things the panel cannot do itself. Log in as `root` first, the same way as for the install: [Installing the panel](install.md).

## Resetting your password

Use this when you cannot sign in and no reset mail arrives. The panel sends mail through the server's own mail relay. Mail may not arrive when the relay is not set up, when the sender address is rejected or when the domain's DNS records (such as SPF) are missing.

Run this with your email and a new password that follows the password rules in [First login](first-login.md#using-the-setup-wizard). `panel` is your panel's project name:

```bash
docker exec -i panel docker exec -i -u www-data panelalpha-v1-panel php /app/backend/artisan users:update you@example.com --password='<new password>'
```

You should see `User updated: you@example.com (password=***)`. Add `--email=<new address>` to change the email address instead.

## Allowing another port for the panel

Use this when **Settings** refuses a port other than 443 for the panel address. The server's firewall only opens the ports you allow. Run this, with your panel's project name in place of `panel`:

```bash
pae system:public-routing:allow panel --ports
```

Then save the address again: [The panel address](panel-address.md#moving-the-panel-to-another-address).

## Giving the panel the server's own name

Use this when the panel says the server's own name can only be given by the person who runs the server. That is you. Run this, with your panel's project name in place of `panel`:

```bash
pae system:public-routing:allow panel --host-domain
```

Then save the address again.

## Certificates for shared names

Many servers share the names under `panelalpha.direct`, `panelalpha.online`, `sslip.io` and `nip.io`. Let's Encrypt allows only 50 new certificates a week for each of them, so the engine does not request certificates for them. A domain of your own does not have this limit.

To let the engine request them anyway, run:

```bash
pae settings:set ssl_shared_zone_issuance 1
```

## When the panel does not know its own project

Use this when the panel says `The panel does not know its own Engine project, so the domain cannot be registered.` The panel was installed without the value that names its own project. Use the panel's project name in place of `panel` (`pae domain:list` shows it next to the panel's address):

```bash
TOKEN=$(pae api:token:create self-project -s --expires=1h)
curl -k -X PUT https://127.0.0.1:2011/api/projects/panel/env-vars \
  -H "Authorization: Bearer $TOKEN" -H 'Content-Type: application/json' -H 'Accept: application/json' \
  -d '{"env_vars":{"PANELALPHA_SELF_PROJECT":"panel"},"apply":"none"}'
pae project:rebuild --project=panel
```

When the rebuild has finished, delete the token. `pae api:token:list` shows its id, and `pae api:token:delete <id>` removes it. Then save the panel address again.

## Visitor countries

The **Visits By Country**, **Continent** and **Region** cards stay empty until you download the geolocation database:

```bash
pae geolocation:database update
```

## Protected projects

A protected project cannot be deleted, stopped or redeployed from the panel. The panel shows a message that says so. To lift the protection, run this with the project's name:

```bash
pae project:settings:unset protected --project=<project>
```

## Backup stores made on the server

A backup store made on the server with `pae backup:container:create`, or by an earlier panel (such as `panel-local-…` in `/opt/panelalpha/backups`), is kept and used by the panel. The panel never deletes it. If you remove it in **Settings**, the panel only stops using it. Settings and backups: [Backups](../looking-after-a-project/backups.md).
