---
title: "First login"
description: "At your first visit the setup wizard creates your account. This page shows the wizard and how to sign in later."
nav:
  group: Getting Started
  order: 3
---

# First login

At your first visit the setup wizard creates your account.

The panel works over HTTPS only. Open `https://panel.example.com`, not `http://`.

## Using the setup wizard

The wizard shows when the panel has no account yet. Every address on the panel leads to it until you finish. It creates your account.

1. On the first screen, `A few steps to your first deploy.`, select **Start Now**.

   ![The setup wizard of a new panel: A few steps to your first deploy and Start Now, the Sign in step with Connect with PanelAlpha Hub and Create a local account instead, the Server name step filled with Northwind Server, Jane Doe, you@example.com and a hidden password, then You're all set and Open PanelAlpha](../../assets/single-server/first-login-wizard.gif)

2. On the **Sign in** step, choose how you sign in:
   - **Connect with Hub** uses your PanelAlpha Hub account. The account is kept in the Hub, not on this server, and you can sign in to it with GitHub or Google. Follow [Connecting with PanelAlpha Hub](#connecting-with-panelalpha-hub) below.
   - **Create a local account instead** makes an account that works on this server only. Go on with step 3.
3. On the **Server name** step, fill in the form:
   - **Server name** is a label for this server. It starts as `My server`.
   - **Your Name** is optional.
   - **Email** is your login.
   - **Password** and **Confirm password** must match. The password needs at least 8 characters, with 1 lowercase character, 1 uppercase character, 1 number and 1 symbol.

4. Select **Continue**.
5. On the **Done** step, which reads `You're all set.`, select **Open PanelAlpha**.

You are signed in and land on the project list.

## Connecting with PanelAlpha Hub

1. On the **Sign in** step, select **Connect with Hub**.
2. Sign in on PanelAlpha Hub and approve the connection. You come straight back to the wizard.
3. The **Name your server** step shows `Signed in as <email>`. Enter a **Server name** and continue. **Use another account** starts again with a different Hub account.
4. On the **Done** step, select **Open PanelAlpha**.

Your Hub account is now the panel's account. You sign in later with **Sign in with PanelAlpha** on the login page. What the connection does: [Settings](../the-panel/settings.md#panelalpha-hub).

If you finish the wizard with a local account, you can connect later in **Settings**.

## Signing in later

1. Open the panel address.
2. Enter your email address and select **Log In**.
3. On the next screen, enter your password and select **Log In**.

On the password screen:

- **Remember Me** keeps you signed in.
- **Forgot password?** sends you a reset link.
- The screen shows the address you entered. If you mistyped it, select **Back**, correct the address and select **Log In** again.

## What you see first

The project list shows the projects the panel finds on the Engine. On a new Engine it is empty. The panel's own project is not in the list, and neither is another panel's.

![The top bar after the first login: the user menu opens with Light Mode, Account Details, Tasks, Settings, Join Our Community and Log Out](../../assets/single-server/first-login-landing.gif)

Open the user menu to reach **Tasks** and **Settings**: [Settings](../the-panel/settings.md). To create your first site: [Create a project](../projects/create-a-project.md).

Something not working? The messages you may see on this page and what to do about them: [Troubleshooting: The panel and your account](../troubleshooting/panel-and-account.md#first-login).
