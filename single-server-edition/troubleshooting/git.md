---
title: "Git"
description: "Problems when you connect a repository, save a token, turn on Auto-deploy or deploy a Git project, with what each message means and what to do."
nav:
  group: Troubleshooting
  order: 3
---

# Git

Problems when you connect a repository, save a token, turn on Auto-deploy or deploy a Git project, with what each message means and what to do.

## Git

Each message belongs to the card it starts from.

**Advanced > Git > Connect repository**

For an HTTPS repository, the message shows under **Repository URL** or **Access token** until you change that field or select **Connect repository** again. Nothing is connected when you see one.

**"The repository refused this token. Check the token and that it can read this repository."**, **"The repository was not found. Check the repository address and that the token can read it if the repository is private."**, **"The host cannot be reached from the panel."** and **"Could not reach <host>, so the repository cannot be read and nothing was connected. Check the address, and that this server can reach it."** Check the address and the token. The address must be the one from your Git provider, and the token must be able to read the repository. The panel does not accept an address on this server or its own network.

**"The repository has no branch to connect yet. Push a commit to it first, then connect again."** The repository is empty, so there is nothing to deploy. Push a commit, then connect again.

**"The repository could not be read: <git's reason>"** or **"Could not connect the repository. Check the URL and token."** Read the reason Git gives and fix what it names. If there is none, try again.

An SSH address shows its refusal under the deploy key instead. A **deploy key** is a key you add to the repository so the panel can read it.

- **"The repository did not accept the key. Check that you added it to this repository."** The key is not on the repository yet. Add it.
- **"Could not connect. Check the address and the key."** This covers every other reason. Check the address and the key.

**Advanced > Git > Access token > Manage > Save token**

**"The repository refused this token. Check the token and that it can read this repository."** and **"The repository was not found. Check the repository address and that the token can read it if the repository is private."** The causes are the same as above, and the token is not saved. The first message also appears when your provider does not know the token, even if the repository is public.

**"The repository could not be reached to check the token. Try again later."** The Git provider did not answer, so the token was not checked or saved. Try again later.

**"The host cannot be reached from the panel."**, **"The repository address is not allowed. Check the repository address; the token was not tested."** and **"The panel cannot check a token against this project's repository address, so the token was not saved."** Check the repository address of the project. Use the address from your Git provider.

**Advanced > Git > Auto-deploy on push**

**"Could not register a webhook with the Git provider. Check that your token can manage webhooks for this repository."** A webhook is how your Git provider tells the panel about a new push. The token cannot manage webhooks. Make a token with that permission and save it.

**"The repository credentials are missing; update the git credentials first."** The project has no access token, and Auto-deploy needs one to add the webhook. Select **Manage** in the **Access token** card and save a token. Then turn Auto-deploy on again.

**Deploying**

**"A deploy of this project is already running."** or **"The project is busy with another operation. Please try again later."** Wait for it to end, or cancel the deploy in its Deploy Logs window.

**"Deploys are only available for container projects."** The project is a WordPress or other PHP site, and for those the panel only pulls and pushes files. To deploy from a repository, create a project from Git: [Projects](../projects/projects.md).

**"This project cannot be deployed from git. Create a new project instead."** This appears in the create-project wizard. Create a new project from Git.

**"Git is not available for this server yet."** The engine does not support Git for this project.

More about this: [Git](../projects/git.md).
