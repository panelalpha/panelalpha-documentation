---
title: How to turn telemetry off
description: "Telemetry is on by default, so reports do leave your VPS unless you change something. What it is, why it is there and what it gives you: Telemetry."
nav:
  group: Getting Started
  order: 7
---

# How to turn telemetry off

Telemetry is on by default, so reports leave your VPS unless you change something. What it is, why it is there and what it gives you: [Telemetry](what-is-collected.md).

The usual switch is a `pae` command. It takes effect immediately. You do not restart the engine. The options further down are lines in `/opt/panelalpha/shared-hosting/.env-core`. They do nothing until you reload the engine: [Change a setting](install.md#change-a-setting).

## Stop sending anything

```bash
pae telemetry:disable
```

What this does: stops reports from being sent, tells monitoring to stop sending emails about this VPS and refuses new bug reports. The **scheduled** six-hourly health check does not run.

What you should see: `Telemetry sending disabled.`

Check it worked:

```bash
pae telemetry:status
```

You want `Sending reports: no` and `Bug reports: no`.

To turn sending back on:

```bash
pae telemetry:enable
```

**What you give up:** the six-hourly check is what tells you a site broke *after* it deployed, and it stops running. Checking a site yourself still works. Ask your assistant, or run `pae project:deploy:check` or `pae project:health:report --local`. These on-demand checks are local. They are not telemetry.

## Keep the automatic reports, refuse bug reports

```bash
TELEMETRY_BUG_REPORTS=false
```

Then reload the engine. Automatic deploy and health reports carry on. The engine refuses to file a bug report.

Automatic reports are measurements the engine took, with no free text and no contact details. A bug report contains a description somebody typed and possibly an email address. If you accept the first and not the second, use this setting.

## Keep the checks, send nothing

```bash
PANELALPHA_MONITORING=
```

Leave the value empty, then reload the engine. The engine still produces reports and records them on your VPS, but has nowhere to send them. The six-hourly health check still runs, so you keep the "this site broke since it deployed" alerts. Choose this if you object to data leaving your VPS, not to the checks themselves.

## What turning it off does not do

**It does not delete anything already on your VPS.** The queue and the local log stay where they are.

**It does not affect the error messages you rely on.** The engine produces the plain-sentence explanations in your deploy logs locally. They have nothing to do with telemetry: [Reading errors](reading-errors.md).

**It does not stop the installer's own report.** The one short report the installer sends when an install or update finishes does not depend on this setting: [What it contains](what-is-collected.md#the-installers-own-report).

**It does not disable on-demand health checks.** The scheduled six-hourly check is telemetry, so it stops when sending is fully off. Checking a site yourself still works:

```bash
pae project:deploy:check <project>
pae project:health:report --local
```

`--local` records the result without sending it. This is how you get a verdict on a server with telemetry off.

## Troubleshooting

**`pae telemetry:status` still shows sending as on.**
Use `pae telemetry:disable`. Editing `TELEMETRY_ENABLED=false` in `.env-core` is not enough after a normal install. The engine already recorded sending as on, and that recorded choice wins until you run the command.

**I set `TELEMETRY_ENABLED=false` but the log file is still growing.**
This is expected. Turning sending off stops reports from being sent. It does not stop the local log from being written.

**I want to report a bug but filing is refused.**
Filing needs telemetry on, because otherwise nothing would deliver the report. Run `pae telemetry:enable` for that one report, or contact PanelAlpha directly at [manage.panelalpha.com/contact](https://manage.panelalpha.com/contact).
