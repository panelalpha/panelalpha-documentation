---
title: "First login"
description: "The first login is either a sign-in with the account you passed at install or the setup wizard, which creates the account. You get the sign-in when you..."
nav:
  group: Getting Started
  order: 3
---

# First login

The first login is either a sign-in with the account you passed at install or the setup wizard, which creates the account. You get the sign-in when you passed `PANELALPHA_USER_EMAIL` and `PANELALPHA_USER_PASSWORD`: [Installing the panel](install.md).

The panel works over HTTPS only. Open `https://panel.example.com`, not `http://`.

## Signing in with the account from the install

1. Open the panel address.
2. Enter the email address you passed at install and select **Log In**.
3. On the next screen, enter the password you passed at install and select **Log In**. **Remember Me** keeps you signed in, and **Forgot password?** sends you a reset link. The screen shows the address you entered. If you mistyped it, select **Back**, correct the address and select **Log In** again.

The wizard does not show. You land on the project list.

## Using the setup wizard

The wizard shows when the panel has no account yet. Every address on the panel leads to it until you finish.

1. On the first screen, `A few steps to your first deploy.`, select **Start Now**.

   <!-- TODO screenshot: assets/first-login-wizard-start.png. Show the first wizard screen with the text "A few steps to your first deploy." and the Start Now button. Crop to the wizard card. -->

2. On the **Sign in** step, choose how you sign in:
   - **Connect with Hub** uses your PanelAlpha Hub account. The account is kept in the Hub, not on this server, and it can sign in with GitHub or Google. Follow [Connecting with PanelAlpha Hub](#connecting-with-panelalpha-hub) below.
   - **Create a local account instead** makes an account that works on this server only. Go on with step 3.
3. On the **Server name** step, fill in the form:
   - **Server name** is a label for this server. It starts as `My server`.
   - **Your Name** is optional.
   - **Email** is your login.
   - **Password** and **Confirm password** must match. The password needs at least 8 characters, with 1 lowercase character, 1 uppercase character, 1 number and 1 symbol.

   <!-- TODO screenshot: assets/first-login-wizard-account-form.png. Show the Server name step with Server name, Your Name, Email, Password and Confirm password filled with example values (you@example.com). Hide the password characters. No real customer data, tokens or passwords. -->

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

## What you see first

The project list shows the projects the panel finds on the Engine. On a new Engine it is empty. The panel's own project is not in the list, and neither is another panel's, whether or not you set `PANELALPHA_SELF_PROJECT`.

<!-- TODO screenshot: assets/first-login-empty-project-list.png. Show the project list of a new Engine with no projects and the Create First Project button. Include the user menu so the reader sees where Tasks and Settings are. -->

Open the user menu to reach **Tasks** and **Settings**: [Settings](../the-panel/settings.md). To create your first site: [Create a project](../projects/create-a-project.md).

## Troubleshooting

**"The panel must be opened at its configured address (APP_URL) to finish the setup."** You opened the panel at another address, such as its IP. Open it at the domain you gave to `pae project:create`, over `https://`.

**The wizard does not accept the password.** It is missing one of the rules in step 3. Add what is missing.

**The wizard is gone and you see a sign-in page.** Someone finished the setup first, or the install created the account. Sign in with that account.

**The sign-in page says the account could not be created.** The install refused the values, and the panel keeps the setup wizard closed. The deploy log names the rule that failed: [Installing the panel](install.md#when-the-account-cannot-be-created).

**The browser warns about the certificate.** The panel address has a self-signed certificate. That is a name under `panelalpha.direct` other than the server's own name, an Engine without a public address, or a domain of your own whose DNS record does not point at the server yet. Use a domain of your own that points here: [The panel address](panel-address.md).
