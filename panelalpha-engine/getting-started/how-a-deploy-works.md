---
title: How a deploy works
description: "A deploy is the process the engine runs to put a project online. This page lists its steps, what you do next and how rebuilds work."
nav:
  group: Getting Started
  order: 4
---

# How a deploy works

When you ask your assistant to put a project online, the engine runs one process from start to finish. That process is a **deploy**. You stay in the chat. You do not have to pick a webserver, write a Dockerfile or create a database first.

```text
Deploy my application https://github.com/org/app on this PanelAlpha Engine.
```

What this does: the assistant creates a project and starts a deploy.

What you should see: progress in the chat, then a web address to open.

## What happens after you ask

**1. A project is created.** A project is one application or website on your VPS. It gets its own isolated container, so a broken app cannot take the others down. The deploy log names each step as it starts: the project's folders and user, its compose file, its container and its disk quota. It also says how long the project's own Docker took to answer. If Docker never answers, the log warns and the deploy goes on.

**2. The engine fetches the code.** For git, it clones the repository and fetches its submodules. If a submodule cannot be fetched, the deploy fails and the message says why. For a zip, it unpacks the files you pointed at. A private repository needs a git token first. You paste it on a page the assistant sends you, not in the chat: [Connecting with Git](../capabilities/connecting-with-git.md).

**3. It works out what the application is.** It can be Laravel, Next.js, Django, Rails, Go, WordPress, a plain `Dockerfile` or `Containerfile` and many more. The engine also reads which language versions your project asks for. The file that identifies the application (`package.json`, `composer.json`, `go.mod`, and so on) has to sit at the top level of the repository: [What your repository needs](../supported-projects/what-your-repo-needs.md).

**4. It builds and starts the application.** The engine installs the dependencies, builds the application and starts it inside that container.

**5. It gives the project an address and HTTPS.** The engine's webserver sends that hostname to the container and requests a free Let's Encrypt certificate. If you did not name a domain, the engine picks a public name and tells you which. **Name a domain only if it already points at this VPS.** A domain that is not set up yet turns a working deploy into a failed one: [Looking after your project](looking-after-your-project.md).

**6. It checks that the address is really your application.** The engine opens the project from inside the account and checks that your app answers, not a placeholder page or an error.

If the project needs a database or another service that does not come up healthy, the deploy fails. It quotes what that service's own health check answered, so a wrong password or a slow start is named, not only the start-up output.

**What counts as serving.** These do not count as serving:

- A page that answers with an error.
- An app that answers once and then keeps restarting. The engine confirms a restart loop before it reports one.
- An app's own error page served with a success code (200).
- The engine's "Project not configured" page, which shows when the engine could not recognise the repository. That page answers 404 on every address, and the project counts as not healthy.

If nothing answered, or the hostname cannot be opened from the internet, the deploy finishes as **partial**, not successful. It says in plain sentences what is wrong. See [Monitoring and logs](../capabilities/monitoring-and-logs.md) and [When a deploy fails](what-happens.md).

**Refused before it starts.** The engine refuses a deploy up front when the container runtime it starts projects with is not running on the host. No project container could start or build. The message says so and names the fix. Run this on the VPS as `root`, then deploy again:

```bash
systemctl restart sysbox
```

**Railpack only when asked.** The engine never chooses the general build, Railpack, on its own. Ask for it in the request, for example `Deploy https://github.com/org/app as railpack`. The choice is not saved, so every redeploy has to ask for it again. A Railpack redeploy that fails leaves the running app as it was.

**A push to your git host cannot ask for Railpack, so it replaces a Railpack project with the "Project not configured" page.** For such a project, give the repository a `Dockerfile`, a Compose file or a `panelalpha.yaml`: [How detection works](../supported-projects/how-detection-works.md).

## What you do next

Open the address in a browser.

- **It opens and looks right.** The first deploy is done. Add your own domain, take a backup or make a test copy: [Looking after your project](looking-after-your-project.md).
- **The address ends in `.local`.** Nobody on the internet can open that project. The application is fine. It has no public name. Point DNS at your VPS, then add a domain: [Domains and HTTPS](../capabilities/domains-and-ssl.md).
- **The deploy failed, or came back "partial".** [When a deploy fails](what-happens.md). Each failure, warning and refusal carries a short code and a list of what to do next. Your assistant reads and acts on them: [Codes and hints](reading-errors.md#codes-and-hints).
- **It opens but is not your application** (a placeholder, a directory listing, an error page). Ask:

```text
This project does not open properly. Read the deploy log, check what it is
actually serving, and tell me what is wrong.
```

## A project that waits for its files

A container project created without a repository has nothing to build yet. Its first deploy sets the account up, gives the project its address and stops on `Environment ready, waiting for project files`. Nothing has failed. The project waits for the files you send. A project that never receives any keeps waiting.

While it waits, every client sees the same thing. The deploy history lists one deploy with status `running`, kind `install` and source `template`. It has no task and no finish time, and its log ends on that line. No process runs behind it, so there is nothing to cancel.

The deploy that brings the files, an archive upload or a rebuild, does not continue that one. It opens a deploy of its own, with its own id and log. The waiting deploy is closed first as `cancelled`, with the sentence `The project waited for its files, and a later deploy brought them.` The history then lists both: the install that waited, and the deploy that came next. `cancelled` is the engine's word for a deploy that ended before it finished. Nothing is wrong with the project.

## Deploying again later

At the end of a successful deploy the engine saves the exact plan it used. A **rebuild** replays that saved plan, so it runs what shipped last time and does not decide again from scratch. Once a project is deployed from a repository, updating it is a rebuild.

```text
Rebuild this project.
```

If the project is connected to git, ask the assistant to pull the latest commit. That pull rebuilds the site: [Connecting with Git](../capabilities/connecting-with-git.md). A rebuild of such a project clones its branch again first, so files you changed in `~/project` on the server are replaced by what the repository holds. A project deployed from an archive or written file by file is rebuilt from the files already in `~/project`.

The version already online keeps serving while a rebuild runs. The new version starts beside it. The site moves over only when the new version answers with a page. A server error or an empty page does not count. If the new version never answers, the deploy fails with what it printed and the old version stays online.

If the new version answers beside the old one but not on its own port once it has replaced it, the engine starts the old version again. The site moves to it once it answers. The deploy fails and says so. The site never moves to a version that does not answer. While nothing else answers, the new version's second copy keeps serving. If the old version cannot be started again, the second copy serves until the app answers on its own port.

Proxy rules you added yourself that point at the app's port move with the site. They move to a new port when the new version listens on another one. Rules to any other port stay as they are.

Some projects cannot run two copies side by side: one behind a Cloudflare tunnel, or a repository compose file with several services. The engine replaces those in place, with a short gap, and the deploy log says why.

If the engine stops in the middle of a rebuild, the deploy is marked failed within a minute or two. Its log says which version serves and why:

- The old version serves while it still runs or can be started again.
- The new version serves only when it passed its health check and still answers, or when nothing of the old one is left.
- When the chosen version does not answer and the new version's second copy does, the copy keeps serving until the chosen version answers.

## Cancelling a deploy

```text
Cancel the deploy that is running on this project.
```

A cancel ends the deploy at any step, including the final check. It never takes down a version that was already online.

- A cancelled rebuild removes only what it started itself.
- A project that was stopped before stays stopped.
- If the engine could not read what was running when the rebuild started, the cancel removes nothing, because what the rebuild started may be what serves the site.
- The project keeps the deployment status of the version still online. Only the deploy's own log says it was cancelled.
- A cancelled first deploy, with nothing online yet, counts as failed.

## Past deploys

The engine keeps the log of the last 10 deploys of each project. The server operator can change this limit with `DEPLOY_LOG_KEEP` in `/opt/panelalpha/shared-hosting/.env-core`. When a new deploy starts, the engine deletes older logs, except the newest install. The deploy that first set the project up stays, however many deploys followed it.

```text
List the recent deploys of this project and show the log of the last failed one.
```

Each deploy is listed with its result and the branch and commit it ran. The list also says what started it: your assistant, your own software through the API, a command on the server or a push to your git host.

## More requests to try

**Mode** is the lowest permission mode that allows it: read, modify or full ([Decide what the assistant may do](../connecting-your-ai/your-assistant.md#decide-what-the-assistant-may-do)). Examples assume a repository project named `tshop`.

**Why a deploy failed, and what to do next**

```text
The last deploy of tshop failed. Read its log, tell me the code and the reason and list what I can do about it.
```

Mode: read. Tools: `deploy_list`, `deploy_log_get` or `deploy_log_read`. Check: the answer quotes the failure sentence and its code, and lists the next steps the log gives, not guesses of its own.

**A project that serves the placeholder**

```text
tshop does not open properly. Check what it is serving and whether the engine counts it as healthy.
```

Mode: read. Tools: `app_health_check`, `project_inspect`. Check: for a project the engine could not recognise, the answer says it serves the "Project not configured" page with a 404 and that this is not healthy, and it names a way to fix it, such as adding a `Dockerfile`.

**The preparing stage in the log**

```text
Deploy https://github.com/org/app as tshop and show me what the preparing stage did in the deploy log.
```

Mode: modify. Tools: `project_create`, `task_get`, `deploy_log_get`. Check: the log lists the account folders, the compose file and the wait for the account's Docker before the build starts.
