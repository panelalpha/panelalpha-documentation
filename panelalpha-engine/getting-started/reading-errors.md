---
title: What the error means
description: When a deploy fails, the engine turns the build output into one plain sentence. This list shows those sentences and what to do about each.
nav:
  group: Getting Started
  order: 9
  title: Reading errors
---

# What the error means

When a deploy fails, the engine turns the build output into one plain sentence. The tables below list those sentences and what to do about each.

Start with your assistant. It reads the same log, finds the same causes and can usually apply the fix:

```text
That deploy failed. Read the deploy log and tell me what went wrong, then fix
what you can.
```

If PanelAlpha already knows the fix for this failure, the deploy log ends with `Known problem:`, why it happened and how to fix it. Your assistant sees it too. See [What it gives you](what-is-collected.md#what-it-gives-you).

## Codes and hints

Every failure, warning and refusal also carries a short **code**, such as `host_disk_low`, and a list of **hints**: what to do next. Your assistant reads both and can act on them. On the server, `pae project:deploy:log <project>` and `pae project:create` print each sentence followed by the commands that act on it.

The sentence can change between releases. The code does not, so quote the code when you ask for help.

Come back here to look up a sentence yourself. If no row below matches, the log usually says `A build step failed (exit code N)`. That means the engine did not recognise the cause. It does not mean there is no cause. The command that failed is in the log just above that line. The tables show the common wording, not every sentence the engine can produce.

## Wrong version of something

| The message | What it means | What to do |
|---|---|---|
| This project needs PHP x, but it was built with PHP y | Your `composer.json` asks for a PHP version the engine did not pick. | Check which version your project needs and set it clearly in `composer.json`. |
| This project needs Node … | Your `engines.node` does not match the Node version used. | Set `engines.node` or add a `.nvmrc`. |
| This project needs Go x, but it was built with Go y | Your `go.mod` asks for a different Go version. | Align `go.mod` with a version the engine offers. |
| This project needs the PHP extension … | Your application needs a PHP extension the engine did not install. | Ask your assistant. If the extension is a common one, report it as a bug. |
| This project has to be compiled for Java … | The project asks for a newer Java than the image the engine builds with. | Lower the Java release in the project, or report it if that release is the one you have to use. |

## Dependencies will not install

| The message | What it means | What to do |
|---|---|---|
| This project's Composer dependencies cannot all be installed together | Two packages need incompatible versions of a third. | Fix it in your repository. It would fail on your own machine too. |
| The project's dependencies conflict with each other | The same problem, for npm or yarn. | Fix the versions in `package.json`, then commit the updated lockfile. |
| A required package was missing when the app started | The install did not finish, but the engine tried to start the app anyway. | Read the install section of the log. The real error is further up. |
| Bun lockfile / frozen lockfile | Your `bun.lock` no longer matches `package.json`. | Run bun locally and commit the updated lockfile. |

## Out of resources

| The message | What it means | What to do |
|---|---|---|
| The build ran out of disk space | The project's disk allowance is used up. | Free space, or raise the limit: [Limits](../capabilities/projects.md#limits). |
| Deploy refused before it started: the engine host has … free | The server itself is nearly full (below `DEPLOY_HOST_MIN_FREE`, 3G by default). Nothing was built. | Free disk on the server. `pae system:image:prune` removes base images that no project has used recently. Then deploy again. |
| The deploy was stopped because the engine host has … free / the project uses … over its disk limit | Disk ran out while the deploy was downloading or building. Either the server fell below `DEPLOY_HOST_MIN_FREE`, or the project passed its disk limit. The engine then cleared the project's unused Docker storage. | Free disk on the server, or raise the project's disk limit, then deploy again. |
| The build ran out of memory | The build needed more memory than it gets. It gets the lower of the project's memory limit and the server's build memory (`DEPLOY_BUILD_MEMORY`; by default 8 GB, or half the server's RAM when that is less). This is common with large JavaScript builds. | The `Image build memory` line in the deploy log gives the figure. If the project's memory limit is the lower one, raise it. Otherwise the server operator can raise `DEPLOY_BUILD_MEMORY`. |
| The build ran out of memory in the engine's build container | The build ran on the server, outside the project, and needed more than the server allows one build (`DEPLOY_BUILD_MEMORY`; by default 8 GB, or half the server's RAM when that is less). | Raising the project's memory limit does not help. The server operator can raise `DEPLOY_BUILD_MEMORY`. |
| The Java build ran out of heap / The Node build ran out of heap | The compiler or Node had less heap (working memory) than this project needs. That cap is set inside the engine, not by the project's plan. | Raising the project's memory limit does not fix this. Deploy again after an engine update, or report it: [Tell PanelAlpha the engine got it wrong](what-happens.md#step-5-tell-panelalpha-the-engine-got-it-wrong). |
| Docker Hub temporarily refused further downloads | Docker Hub limits how many downloads one address can make. This is not your fault. | Wait a few minutes and deploy again. To raise the limit for good, give the engine a Docker Hub login: `pae docker-hub:login`. |
| A base image this project asks for could not be downloaded | Your `Dockerfile` names an image that does not exist or is private. | Check the image name and tag in your `Dockerfile`. |
| The image … could not be downloaded: the build could not look up *host* | The build asked the server's DNS for the registry's address and got no answer. This is a fault on the server, not in the project. | Deploy again. If it repeats, the server's DNS needs fixing. |
| The image name "*name*" is not valid: Docker image names must be lowercase | The compose file or `Dockerfile` spells an image with capitals, such as `Owner/app:1`. Docker reads `Owner` as the address of a registry and cannot find it. | Use the lowercase name the message gives. |

## The server, not the project

The deploy is refused before it starts, with a message that begins like this:

```text
The sysbox runtime is not running on the engine host
```

The runtime the engine starts projects with is stopped, so no project can start or build. Restart the runtime as `root` on the VPS, then deploy again:

```bash
systemctl restart sysbox
```

A project that lost its connection to the runtime shows "function not implemented" in its log. In that case, recreate the project's container instead:

```bash
pae project:dind:recover --project=<name> --recreate
```

## The repository

| The message | What it means | What to do |
|---|---|---|
| The repository could not be read | The engine could not access the repository. | Check the URL. If the repository is private, supply an access token. |
| The repository was not found | Wrong address, or private with no access. | Paste the URL into a browser in a private window. If you cannot see it, neither can the engine. |
| The repository's submodules could not be fetched | A submodule is private, no longer exists at the commit the repository records, or sits on a host the git token does not cover. The reason from git follows the sentence. | Give the engine a token that can read the submodule. A private submodule addressed over SSH on another host needs a deploy key. If the application does not need the submodule, name the ones it does need under `git: {submodules: [...]}` in `.panelalpha/panelalpha.yaml`: [Connecting with Git](../capabilities/connecting-with-git.md#troubleshooting). |
| The repository is refused with `host_refused` | The address leads to the engine's own machine or a private network, and the engine does not fetch from those. Or the host name does not resolve, so the engine cannot check it. | The server operator can allow a private git server: [Connecting with Git](../capabilities/connecting-with-git.md#troubleshooting). |

## The application refuses to start

| The message | What it means | What to do |
|---|---|---|
| The application refused to start because one of its settings is missing or still has a placeholder value | Your app needs an environment variable that is missing or still says something like `changeme`. | Set the real value: [Environment variables](../capabilities/projects.md#environment-variables). On a container project the assistant can apply it in place. A value that the build reads needs a rebuild. |
| The service … did not start (…) … Its health check said: … | A service the application depends on, often a database, never became healthy. The quote is the answer its own health check gave. | Read the quote. A refused login points at the password. A timeout points at a service that needs longer to start. The full output is in the deploy log. |
| The application could not sign in to its database | The application's password and the database's password no longer match. | The database keeps the password it was first created with, so changing the setting does not change it. Either set the setting back to the original password, or delete the database's stored data and let it be recreated. |

## Build tooling

| The message | What it means | What to do |
|---|---|---|
| The build needs the git binary | Usually a Git hook installer such as husky. Those cannot run during a hosting build. | Change the `prepare` script in your `package.json` so it works without Git. |
| A package.json "prepare" script failed | The same cause. | The same fix. |
| The project has no "build" script in package.json | The engine expected a build step and there is none. | Add a `build` script, or check whether this project needs one at all. |
| No runnable program was found in this Go repository | The engine could not find a program to start. | Make sure your `main` package is where the engine looks for it. |
| This project embeds files that have to be produced by an earlier build step | Your build expects files that another build step should have generated first. | Commit the generated files, or add a Dockerfile that does both stages. |
| A dependency has to be compiled … needs development headers / no C or C++ compiler / no Python or C toolchain | A native library has to be built during install, and the build image does not have the tools for it. | Use a release that ships a prebuilt binary. If the repository is fine and the engine mishandled it, report it. |
| The build step "…" printed nothing for … and was stopped | That step went silent, usually on a download or network call, and the engine stopped it. | Deploy again. If it stalls at the same point, the problem is in that step. |

## Nothing matched

| The message | What to do |
|---|---|
| A build step failed (exit code N) | The engine did not recognise the cause. Read the log around that point. The failing command is right there. Then ask your assistant. |
| Failed to start app: … | The rest of the sentence is one of the causes above. |

## The site is up but the page is wrong

If your deploy **succeeded** and the page is still wrong, the problem is not a build failure, so it is not listed here. The build worked. Examples are a placeholder, a directory listing, someone else's welcome page or a PHP error.

The "Project not configured" page is the engine's own placeholder for a repository it could not recognise. It answers 404 on every address. The project then counts as not healthy, and the deploy does not report plain success. An app's own error page served with a 200 counts as not serving too. A project of the general build (Railpack) shows the placeholder after a rebuild that did not ask for Railpack again: [How detection works](../supported-projects/how-detection-works.md).

The engine has a separate check for this. It can usually name what is served instead: [What the engine checks](../capabilities/monitoring-and-logs.md#what-the-engine-checks).

## If you think the engine is wrong

Not every failure comes from your repository. If your project is fine and the engine mishandled it, report it: [Tell PanelAlpha the engine got it wrong](what-happens.md#step-5-tell-panelalpha-the-engine-got-it-wrong).

## From the server

Reading the deploy log: [CLI commands](../commands/pae-cli.md#projects-and-deploys).
