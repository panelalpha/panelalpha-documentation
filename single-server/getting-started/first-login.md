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

The wizard shows until the panel has an account. Every address on the panel leads to it until you finish.

1. On the first screen, select **Start Now**.

   :docs-video{src="../../assets/single-server/first-login-wizard.webm" title="The setup wizard of a new panel: Start Now, the Sign in step with Connect with Hub, the Server name step, the Connect AI step where Create another token makes a token for Claude Code, and the last step You are all set with Create your first project"}

2. On the **Sign in** step, choose how you sign in:
   - **Connect with Hub** uses your PanelAlpha Hub account, which you can sign in to with GitHub or Google. Follow [Connecting with PanelAlpha Hub](#connecting-with-panelalpha-hub) below.
   - **Create a local account instead** makes an account that works on this server only. Go on with step 3.
3. On the **Server name** step, fill in the form:
   - **Server name** is a label for this server.
   - **Your Name** is optional.
   - **Email** is your login.
   - **Password** and **Confirm password** must match. The password needs at least 8 characters, with 1 lowercase character, 1 uppercase character, 1 number and 1 symbol.

4. Select **Continue**.
5. On the **Connect AI** step, create a token to connect your AI assistant, or select **Skip for now**: [Connecting your AI assistant](#connecting-your-ai-assistant).
6. On the last step, which reads `You're all set.`, select **Create your first project**.

You are signed in and land on the project list. The create screen for your first project opens at once. The last step also lists your server name, your admin email or Hub account, the panel address, the version (Free) and whether PanelAlpha Hub and your AI assistant are connected.

The wizard has a language switcher in its footer.

## Connecting with PanelAlpha Hub

1. On the **Sign in** step, select **Connect with Hub**.
2. Sign in on PanelAlpha Hub and approve the connection. You come back to the wizard.
3. Enter a **Server name** and continue. **Use another account** starts again with a different Hub account.
4. Go on with the **Connect AI** step, then select **Create your first project** on the last step: [Using the setup wizard](#using-the-setup-wizard).

Your Hub account is now the panel's account. Later, sign in with **Sign in with PanelAlpha** on the login page. What the connection does: [Settings](../the-panel/settings.md#panelalpha-hub).

If you finish the wizard with a local account, you can connect later in **Settings**.

## Connecting your AI assistant

The **Connect AI** step offers a card, **Connect with MCP**. Select **Create token** to open the same token dialog as on the Connect AI page. After you create a token the card reads `AI is connected to this panel.` and **Continue** replaces **Skip for now**. You can also connect later: [Connect AI](../the-panel/connect-ai.md).

## Signing in later

1. Open the panel address.
2. Enter your email address and select **Log In**.
3. Enter your password and select **Log In**.

**Remember Me** keeps you signed in. **Forgot password?** sends you a reset link. If you mistyped the address, select **Back** and correct it.

## What you see first

The project list is empty on a new server. The panel's own project is not in the list.

:docs-video{src="../../assets/single-server/first-login-landing.webm" title="The user menu after the first login: Light Mode, Account Details, Tasks, Settings, Join Our Community and Log Out"}

Open the user menu to reach **Tasks** and **Settings**: [Settings](../the-panel/settings.md). To create your first site: [Create a project](../projects/create-a-project.md).

Something not working? [Troubleshooting: The panel and your account](../troubleshooting/panel-and-account.md#first-login).
