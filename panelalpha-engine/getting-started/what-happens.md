---
title: When a deploy fails
description: Your site did not come up after a deploy. Follow these steps to find the cause and fix it.
nav:
  group: Getting Started
  order: 8
---

# When a deploy fails

Your site did not come up. The build may have stopped with an error. It may have built but the application never started. Or it may have started with the wrong page.

Follow the steps below in order.

**The engine never edits your code.** It tells you what went wrong in plain language. Changing your application is your job or your AI assistant's. It never happens silently on your VPS.

## Step 1: ask your assistant

Most of the time, this is the whole answer:

```text
That deploy failed. Read the deploy log and tell me what went wrong.
```

The assistant reads the log, the error and what the site actually serves. It can then fix many problems directly, such as a wrong version number, a missing setting or a bad start command, and try again.

If you want to read the log yourself, or you have no assistant connected, run:

```bash
pae project:deploy:log <project>
```

## Step 2: read the one sentence

When the engine recognises what went wrong, the log contains a plain sentence that says so. It is a sentence, not a stack trace. For example:

> [!NOTE]
> This project needs PHP 8.2, but it was built with PHP 8.1

> [!NOTE]
> The build ran out of memory

> [!NOTE]
> The repository was not found

The full technical output is still there underneath if you need it. Start with the sentence.

Each sentence also has a short code and a list of what to do next. Your assistant and `pae project:deploy:log` show them. A refusal, for example when the engine's container runtime is stopped, comes with the fix: [Codes and hints](reading-errors.md#codes-and-hints).

Common sentences, and what to do about each, are on [Reading errors](reading-errors.md). If the sentence in your log is not there, still start from it. That page covers the usual cases, not every wording the engine can produce.

## Step 3: work out whose problem it is

Finding the right cause saves time. There are three cases.

**Your repository is wrong.** Examples: no `package.json` at the top level, no `start` script, a private repository with no access token, or a dependency that cannot install. Fix the repository and deploy again. Most failures are of this kind, and you can fix them yourself.

**Your VPS ran out of something.** It ran out of disk or memory, or hit a download rate limit. Raise the project's limits or free up space: [Limits](../capabilities/projects.md#limits).

**The engine got it wrong.** Your repository is fine and the engine mishandled it. It identified your application as the wrong kind, or served a placeholder page over a complete site. You report this case. See [Step 5](#step-5-tell-panelalpha-the-engine-got-it-wrong).

## Step 4: if the project deployed but looks wrong

A deploy that says "success" can still leave a broken page. This is not a build failure, so it does not appear in the error list. The build worked.

A project the engine could not recognise serves its "Project not configured" page with a 404 and counts as not healthy. It does not end as a plain success.

The engine checks what your site actually serves and can usually name the problem: [What the engine checks](../capabilities/monitoring-and-logs.md#what-the-engine-checks).

## Step 5: tell PanelAlpha the engine got it wrong

Two different things can reach PanelAlpha.

**Automatic reports.** If telemetry is on, the engine sends an anonymous summary of the failure. It lets PanelAlpha see that a kind of failure happens across many installs. You do nothing, and nobody replies to you.

**A bug report you file.** Use it when the engine believes it did the right thing and did not. Nothing automatic catches that, because from the engine's point of view nothing failed.

```text
File a bug: the engine treated this project wrongly. Here is what happened,
and here is what should have happened instead.
```

You describe the problem. The engine attaches the evidence itself: what it identified your application as, what your site actually returns, the last deploy and the end of that deploy's log. If the assistant cannot file the report, use the VPS command below.

From the server:

```bash
pae telemetry:bug-report <project>
```

The command asks you for a title and a description, gathers the same evidence and queues the report. Add `--contact you@example.com` if you want a reply. Add `--dry-run` to see what would be sent without sending it.

**If telemetry is switched off, you cannot file a report.** Turn it back on with `pae telemetry:enable`, or contact PanelAlpha directly at [manage.panelalpha.com/contact](https://manage.panelalpha.com/contact).

What a report contains and what is stripped out of it: [Telemetry](what-is-collected.md#bug-reports).

## Step 6: the fix arrives as an update

When PanelAlpha fixes something on its side, such as better recognition of a framework or a repaired build recipe, the fix reaches your VPS through an engine update. [Updating](updating.md).

## What the engine will never do

- Change your source code to make a build pass
- Open a support ticket in your name
- Modify your application on its own

The engine does retry a few situations by itself during the same deploy, such as running low on disk. A retry is not an edit to your code.

## A failed create leaves nothing behind

If a project fails while being created, the engine deletes the account it had just started to make. No half-built project is left to clean up, and you can use the name again.
