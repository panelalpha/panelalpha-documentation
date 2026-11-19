---
title: "First login"
description: "At your first visit the setup wizard creates your account. This page shows the wizard and how to sign in later."
nav:
  group: Getting Started
  order: 3
---

# First login

At your first visit the setup wizard creates your account. Open `https://panel.example.com`, not `http://`. The panel works over HTTPS only.

## Using the setup wizard

The wizard shows until the panel has an account. Every address on the panel leads to it until you finish. It has a start screen and four steps. The list of steps is on the left: **Sign in**, **Server name**, **Connect AI** and **Done**. The language list at the bottom of the screen changes the language of the wizard.

:docs-video{src="../../assets/single-server-edition/first-login-wizard.webm" title="The setup wizard of a new panel: Start Now, the Sign in step with the Connect with PanelAlpha Hub card and the link Create a local account instead, Connect with Hub, then the Name your server step with Connected to PanelAlpha Hub, Signed in as you@example.com and the Server name My server, and Continue to the Connect AI step"}

### The start screen

The first screen reads `A few steps to your first deploy.` Select **Start Now** to begin.

### Step 1: Sign in

The step asks `How do you want to sign in?` It offers two choices.

- **Connect with Hub.** The card is marked **Recommended**. You sign in with your PanelAlpha Hub account, which you can open with GitHub or Google. The card says what you get: the account is kept in the Hub and not on this server, one login works for many servers, you can sign in with GitHub or Google, and you can install verified apps from the PanelAlpha GO! catalog. A small picture shows this server joining your Hub account. Select **Connect with Hub** and follow [Connecting with PanelAlpha Hub](#connecting-with-panelalpha-hub).
- **Create a local account instead.** The link is under the card. The account works on this server only. Go on with [Creating a local account](#creating-a-local-account).

### Step 2: Server name

What you see here depends on your choice in step 1.

After **Connect with Hub**, the step is called `Name your server`. A green box reads `Connected to PanelAlpha Hub` and `Signed in as` followed by your email. **Use another account** starts again with a different Hub account. Below it, **Server name** is a label for this server. Select **Continue**.

After **Create a local account instead**, you see the form in [Creating a local account](#creating-a-local-account).

### Step 3: Connect AI

The step is called `Connect your AI assistant`. It has one card, **Connect with MCP**, marked `Works with any MCP client`. MCP is the way AI tools talk to other programs. The card says what an assistant can do: manage projects, domains and backups, check logs, errors and server use without opening the panel, and lose its access at any time in Connect AI. It also shows the **MCP server address** of this panel with a copy button.

- **Create token** opens the dialog that makes a token for your tool. Follow [Connecting your AI assistant](#connecting-your-ai-assistant).
- **Documentation** opens the guide for this step in a new tab.
- **Skip for now** goes to the next step without a token. You can connect later in **Connect AI**.

After you create a token the card reads `AI is connected to this panel.` **Create another token** makes one more, and **Continue** takes the place of **Skip for now**.

:docs-video{src="../../assets/single-server-edition/first-login-wizard-finish.webm" title="The Connect AI step: Create token, the Token Name Claude on my laptop, the new token shown once as dots, the confirmation I have saved my token in a safe place, Continue, then the last step You are all set with the server name, Hub account, panel address, Community version and Connected labels, and Create your first project"}

### Step 4: Done

The last step reads `You're all set.` It lists what you set up:

- **Server name**: the name you chose.
- **Hub account** (after a Hub sign-in) or **Admin email** (with a local account): the address you sign in with.
- **Panel address**: the address of this panel, with a copy button.
- **Version**: `Community`.
- **PanelAlpha Hub**: `Connected` or `Not connected`.
- **AI assistant**: `Connected` or `Not connected`.

After a Hub sign-in, a box named `Included with PanelAlpha Hub` lists what the Hub gives you: one login for all your servers, one-click admin login, app monitoring across your servers and verified apps from the PanelAlpha GO! catalog. Without the Hub, a note says you can connect it later in **Settings**. **Documentation** opens the docs in a new tab.

Select **Create your first project**. You are signed in and land on the project list. The create screen for your first project opens at once.

## Creating a local account

A local account works on this server only. On the **Server name** step, fill in the form:

- **Server name** is a label for this server.
- **Your Name** is optional.
- **Email** is your login.
- **Password** and **Confirm password** must match. The eye icon in each field shows or hides what you typed. While you type, a list shows which rules the password still needs: 1 uppercase character, 1 lowercase character, 1 number, 1 symbol and 8 characters.

**Connect with Hub instead**, under the title, goes back to the Hub card. **Back** returns to step 1. Select **Continue** to go on to step 3.

:docs-video{src="../../assets/single-server-edition/first-login-wizard-local-account.webm" title="The Name your server step of a local account: the link Connect with Hub instead, the Server name My server, Your Name Jane Doe, Email you@example.com, and the Password field with the list of rules, 1 uppercase character, 1 lowercase character, 1 number, 1 symbol and 8 characters"}

## Connecting with PanelAlpha Hub

1. On the **Sign in** step, select **Connect with Hub**.
2. Sign in on PanelAlpha Hub and approve the connection. You come back to the wizard.
3. Enter a **Server name** and select **Continue**. **Use another account** starts again with a different Hub account.
4. Go on with the **Connect AI** step, then select **Create your first project** on the last step.

Your Hub account is now the panel's account. Later, sign in with **Sign in with PanelAlpha** on the login page: [Signing in](signing-in.md#signing-in-with-panelalpha-hub). What the connection does: [Settings](../the-panel/settings.md#panelalpha-hub).

If you finish the wizard with a local account, you can connect later in **Settings**.

## Connecting your AI assistant

On the **Connect AI** step, select **Create token** on the **Connect with MCP** card. This is the same token dialog as on the Connect AI page.

1. Enter a **Token Name**, for example `Claude on my laptop`, and select **Create**.
2. The token is shown once. Copy it and keep it in a safe place. You cannot see it again.
3. Tick `I have saved my token in a safe place` and select **Continue**.

The card now reads `AI is connected to this panel.` You can also connect later: [Connect AI](../the-panel/connect-ai.md).

## Signing in later

1. Open the panel address.
2. Enter your email address and select **Log In**.
3. Enter your password and select **Log In**.

**Remember Me** keeps you signed in. **Forgot password?** sends you a reset link. If you mistyped the address, select **Back** and correct it. Every way to sign in, step by step: [Signing in](signing-in.md).

## Next steps

To create your first site: [Create a project](../projects/create-a-project.md). The user menu at the avatar holds **Tasks**, **Settings** and your account: [Around the panel](../the-panel/around-the-panel.md).

Something not working? [Troubleshooting: The panel and your account](../troubleshooting/panel-and-account.md#first-login).
