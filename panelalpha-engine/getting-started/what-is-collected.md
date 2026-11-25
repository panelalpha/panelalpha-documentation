---
title: Telemetry
description: "Telemetry is reports your engine sends to PanelAlpha about deploys and about projects that later stopped serving themselves."
nav:
  group: Getting Started
  order: 6
  title: What telemetry collects
---

# Telemetry

Telemetry is reports your engine sends to PanelAlpha about deploys and about projects that later stopped serving themselves. It exists so that PanelAlpha can fix in the product a failure that shows up on many installs, instead of treating it as one broken repository. Telemetry is on after install, and reports leave your VPS unless you change that: [How to turn telemetry off](how-to-turn-it-off.md).

```text
Is telemetry on on this engine, and what does it send?
```

What this does: the assistant checks whether reports are being sent, and summarises what a report contains.

What you should see: whether sending is on, and that reports include the public names of your projects, not your source code.

## What it is

A small, automatic record of how a deploy went, plus a later check that the site is still serving itself. Your VPS is identified by a fingerprint, not by name. Reports include the public names of your sites, because a report has to say which site it is about. Nobody reads a report and replies to you.

Telemetry is not any of these:

- **Not visitor analytics.** Nothing about the people who visit your sites is collected.
- **Not crash reporting for the applications you host.** Their errors stay yours.
- **Not a support ticket.** An automatic report does not open a conversation.

There are three kinds of report:

- **Deploy reports** are sent when a deploy finishes, including a successful one.
- **Health reports** are sent when the six-hourly check finds a site that is no longer serving itself. Only broken sites are reported.
- **Bug reports** are the ones you file yourself, when the engine got something wrong. See [Bug reports](#bug-reports).

## Why it is there

A failed deploy on one server could be caused by that repository. The same failure on thirty servers is a bug in detection, in a framework recipe or in a known-app config. Without the reports, the two cases look identical, and PanelAlpha cannot tell which apps to fix next.

The engine reports what happened. PanelAlpha sees the pattern across installs. A platform fix ships as an update. The engine never edits your application. When *your* repository is the problem, you still fix it yourself: [When a deploy fails](what-happens.md).

## What it gives you

Telemetry gives you four things. None of them is a reply in your inbox.

**A product that improves from what happens on every install.** PanelAlpha can see detection misses and recipe failures that repeat. A product fix for them arrives as an engine update, not as a reply to your report.

**A check that a site is still serving itself after the deploy already succeeded.** A deploy log shows one moment. It says nothing about the site a month later, after a database filled up, a certificate lapsed or someone deleted the index page over FTP. Every six hours the engine opens each deployed application and lists the ones that no longer serve themselves. It also checks that the public name for the project serves this application, not a different one. That check does not run when telemetry is fully off. You can still ask about any site whenever you want: [Monitoring and logs](../capabilities/monitoring-and-logs.md). To keep the check without sending anything off your VPS: [Keep the checks, send nothing](how-to-turn-it-off.md#keep-the-checks-send-nothing).

**The fix for a failure PanelAlpha already knows.** The engine sends the report of a failed deploy the moment the deploy fails. If PanelAlpha already has a fix for that failure, it comes back in the answer. The deploy log then ends with `Known problem:` and what to do. The same fix is attached as `problem`, so you or your assistant can apply it straight away. A failed deploy waits up to 5 seconds for that answer. If nothing answers in time, the engine queues the report as usual and the deploy has no fix attached. With telemetry off nothing is sent, so no fix comes back.

**A way to say the engine was wrong.** Sometimes a deploy "succeeded" but the engine mishandled the project. Nothing automatic catches this, because from the engine's point of view nothing failed. You file a bug report and the engine attaches the evidence. See [Bug reports](#bug-reports).

## What is sent, and what is not

Your VPS is identified by a fingerprint, not by name. The engine combines a few facts about the machine and stores a one-way hash of them. The facts themselves never leave your VPS.

The installer also gives the engine a random ID, `APP_UID` in `.env-core`. The engine sends it in the `X-Engine-App-UID` header with every report and every request to PanelAlpha Hub, so both can tell one installation from another. That includes checking for a newer release (`pae system:update --check`). It asks the PanelAlpha Hub for its public release list and sends nothing else. The ID is not derived from anything on the machine. A value that is already set is never replaced, so you can choose it before installing.

A report contains what kind of application it was, how long things took and the project's limits. It also includes the **public website names** the project answers on, such as `shop.example.com` or `blog-4f2a.panelalpha.online`. Those names leave your VPS in readable form, exactly as a visitor types them. A report needs them to say which site it is about. A name the engine gave the project contains the project's name. Some also contain the server's address, such as `blog.203-0-113-7.panelalpha.direct`.

Everywhere else in the report, such as a deploy log line, an error message or a certificate's issuer, three parts of a name are replaced:

- The project's name becomes `<account>`.
- An IP address that the name spells out becomes `<ip>`.
- The server's own name, the one the engine's certificate is issued for, becomes `<server>`.

For a project named `blog`, a log line quoting `blog.203-0-113-7.panelalpha.direct` says `<account>.<ip>.panelalpha.direct`.

A failed or partial deploy report also includes which stage failed and the end of the deploy log, with sensitive values removed. A successful deploy is reported without that log.

Sometimes a deploy tries more than one way to run the project ([When the first match fails](../supported-projects/how-detection-works.md#when-the-first-match-fails)). Or it starts with the one that an earlier deploy fell back to. The report then also lists each attempt: the kind of project, the package manager, and the name of the rule its failure matched, such as `composer-package-not-found`. That holds for a deploy that succeeded in the end too. It carries no log lines.

**None of this is sent:**

- Project names outside the website names above, replaced with `<account>`
- Tokens, passwords, secrets and email addresses found in logs
- Your VPS's own IP address or hostname in readable form, outside the website names above
- Private repository names, which are hashed
- Filenames from your projects
- Your source code

Two addresses can identify a person, and only if you give them. One is a contact on a bug report you file yourself. The other is a notification email you set so that monitoring can write to you when this engine or a site it checks looks down. Neither is filled in unless you type it.

A setting can attach a copy of your source to a failed deploy. It is **off**. Turn it on only if PanelAlpha support asks you to during an investigation. Set it back afterwards.

## The installer's own report

When the installer finishes an install or an update, it sends one short report to `monitoring.panelalpha.com`, whether the run worked or not. The installer sends it, not the running engine, so `pae telemetry:disable` does not stop it. The report holds:

- whether it was an install or an update, when it started and ended, and the exit code
- the engine version it was installing and, when something failed, a short message saying what
- the email you gave with `--email`, if any
- with `--repo`: the branch or tag, the repository address with any user name or token in it replaced by `***`, and `deploy_result`, which is `deployed`, `updated`, `failed` or `not_run`
- `software`, which is `engine`, or `single-server` when `--full-privileges` deployed the PanelAlpha panel

**The repository address is not hashed here.** It is sent as you typed it, so the name of a private repository leaves your VPS in this one report. The report does not contain source code or the values of `--git-token`, `--password` or `--env-var`.

## An email for alerts

This is optional. Monitoring can write to you if this engine, or a site it probes, looks down. The address leaves your VPS.

```bash
pae telemetry:email set you@example.com
```

What this does: stores the address on this engine and sends it to monitoring.

What you should see: `Notification email set to you@example.com`, then a line that preferences were synced.

To read the address currently stored:

```bash
pae telemetry:email get
```

What you should see: the address, or `(not set)`. `pae telemetry:status` shows the same value as `Notify email`. Turning telemetry off tells monitoring to stop those emails: [How to turn telemetry off](how-to-turn-it-off.md).

To see whether sending is on, and what is queued:

```bash
pae telemetry:status
```

What this does: prints whether reports are being sent, and how many are waiting.

What you should see: `Sending reports: yes` on a default install. After `pae telemetry:disable`, `Sending reports: no`. The table also shows `Notify email` and how many site addresses monitoring is asked to probe.

To print one queued report exactly as it would leave your VPS:

```bash
pae telemetry:show
```

What this does: dumps one report from the queue, already redacted.

What you should see: the same JSON that would be posted to PanelAlpha. If the queue is empty: `Nothing queued.`

## Bug reports

File a bug report when your project is fine and the engine mishandled it. Typical cases: the engine identified your application as the wrong type, or it served a placeholder over a complete site.

Ask your assistant:

```text
File a bug: the engine treated this project wrongly. Here is what happened,
and here is what should have happened instead.
```

Or from the server:

```bash
pae telemetry:bug-report <project>
```

What this does: asks you for a title and a description. It then gathers the evidence itself: what it identified your application as, what your site actually returns and the end of the last deploy log.

What you should see: `Bug report queued.` The report is not sent immediately. It goes out with the next delivery, within five minutes.

Two options help:

| Option | What it does |
|---|---|
| `--dry-run` | Shows exactly what would be sent, and files nothing. **Do this once before your first real report.** |
| `--contact you@example.com` | An address support can reply to. Nothing else in a report identifies anyone. |

**The engine refuses to file a bug report if telemetry is switched off**, because nothing would deliver it. Turn telemetry back on with `pae telemetry:enable`, or contact PanelAlpha at [manage.panelalpha.com/contact](https://manage.panelalpha.com/contact).

## Where reports go

The engine sends reports to `monitoring.panelalpha.com` over HTTPS. If you set a notification email, that address goes there too.

If `monitoring.panelalpha.com` cannot be reached, reports wait in a queue on your VPS. They are not thrown away.

## The local record

The engine records every deploy in a log file, **whether or not sending is switched on**. Turning telemetry off stops reports from being *sent*. It does not stop the engine from writing that file.

## Turning it off

[How to turn telemetry off](how-to-turn-it-off.md). The usual command is `pae telemetry:disable`. It takes effect immediately.

## From the server

Status, enable, disable, the notification email, the queue and filing a bug as `pae` commands: [CLI commands](../commands/pae-cli.md#telemetry-and-the-engine-itself).
