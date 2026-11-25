---
title: "Connect to PanelAlpha Hub"
description: "PanelAlpha Hub is the customer portal at https://hub.panelalpha.com. Connect your engine to it with a one-time code."
nav:
  group: Getting Started
  order: 11
---

# Connect to PanelAlpha Hub

PanelAlpha Hub is the customer portal. The engine reaches it at `https://hub.panelalpha.com`. After you connect your engine to your Hub account, the Hub shows the engine's status, version, server capacity and CPU/RAM/disk usage. It also shows your applications, usage charts and monitoring events. If you allow it, the Hub can also open a one-time admin login into your apps.

A licence is optional. You can connect without one and attach a licence later in the Hub.

## Before you start

- The server must be able to open `https://hub.panelalpha.com` (set `PANELALPHA_HUB` in `.env-core` to use another). The engine reports to the Hub. The Hub never connects to your server, so port 2011 can stay closed to it.
- The server clock must be roughly correct.

## Connect

1. In the Hub, click **Connect engine** (Licenses page). Pick a licence or none, and pick the permissions.
2. The Hub shows a command with a one-time code. Run it on your VPS within 10 minutes:

```bash
pae hub:connect WDJB-MJHT
```

The engine connects at once and shows what it connected to:

```
Connect code accepted by https://hub.panelalpha.com.

Account: Acme Ltd (code created by j***@acme.com)
Permissions:
  - Read the engine version, host details and resource usage
  - Read the list of applications and their status
  - Read usage statistics
  - Read your projects' backup history
Warning: the license is registered for a different IP address
Connected to Acme Ltd.
Not your account? Disconnect with: pae hub:disconnect
```

The Hub then shows "Engine connected". A code works once. If it has expired or been used, create a new one.

**Run only a command you created yourself.** A code connects the engine to the account that created it, without asking. If the account shown is not yours, run `pae hub:disconnect`.

| Option | What it does |
|---|---|
| `--ip=203.0.113.10` | The address PanelAlpha Hub shows for the engine, when it should not be the server's main IPv4. Any IP works, a private one behind NAT too. The Hub never connects to the engine. |
| `--replace` | Connect an engine that is already connected. The old connection is dropped only after the new one works. |

### Install and connect in one step

Pass the code to the installer. The new engine connects as soon as it is up:

```bash
curl -fsSL https://get.panelalpha.com/engine | sh -s -- --hub-code WDJB-MJHT
```

On a server that already runs an engine, the same command only connects it. Add `--update-engine` to update the engine first. The option works together with `--repo`. An engine that is already connected stays as it is. To connect it elsewhere, run `pae hub:connect CODE --replace`.

A code lasts ten minutes from when you create it, and a fresh install takes a few of those minutes. Create the code just before you start the installer. If the connect fails, the engine is still installed. The installer prints why. Connect with a new code and `pae hub:connect CODE`.

## What the engine sends to the Hub

Every minute the engine sends the Hub a signed report. It contains:

- The engine version and host details.
- CPU/RAM/disk usage.
- The project list, with each project's status, usage and disk.

When the Hub asks, the report adds each project's backup history. After connecting or after an outage, the engine also uploads its own usage history so the charts have no gaps.

The Hub works out monitoring events from these reports. It holds no token and never calls your server. So it cannot deploy, change settings, read files or credentials, or use the assistant (MCP) interface.

Admin login into apps is a separate permission. It is off unless it was ticked when the code was created. With it, the Hub hands your browser a login ticket that lasts a minute and works once. The browser posts the ticket to the app's own domain, and the engine logs you in as the app's administrator. The engine records every login. `pae hub:status` shows the last ones. Apps created before the engine was updated get the login route within a few minutes of connecting.

The secret that signs the reports is replaced every week, by itself.

An engine connected before reports existed switches to them by itself after the update. Its connection, account and permissions stay. The token the Hub held stops working.

## Check and disconnect

```bash
pae hub:status
```

This shows the account, the permissions, the report interval, the last 20 reports, the last history upload and the last admin logins. When the Hub says it no longer knows the connection, the engine forgets it and stops reporting.

```bash
pae hub:disconnect
```

This stops reporting and forgets the connection at once, then tells the Hub. It works even when the Hub cannot be reached.

## When it does not work

| Message | What to do |
|---|---|
| `The first report to PanelAlpha Hub failed` | The server cannot reach `https://hub.panelalpha.com`, or its clock is off. Fix that, create a new code and connect again. |
| `… is not an IP address` | `--ip` names a host, not an address. Pass an IP, or leave `--ip` out. |
| `That code is not valid any more` | The code expired, was cancelled or was already used. Create a new one in the Hub. |
| `The code expired` | Create a new code in the Hub and run the command within 10 minutes. |
| `Denied on the Hub.` | The code was cancelled in the Hub. Create a new one. |
| `This engine is already connected to …` | Run the command again with `--replace` to move it. |
| `The Hub answered: … (HTTP 403)` from `pae hub:status` | Something in front of the Hub refused the call, such as a firewall or bot rule. The engine changed nothing. If you removed the connection in the Hub, run `pae hub:disconnect`. |
| The Hub shows the engine as unreachable | The engine has not reported for 10 minutes. Check that the server can open `https://hub.panelalpha.com`. `pae hub:status` shows how the last reports went. |
