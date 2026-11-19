---
title: "Your account"
description: "Your account is your sign-in on the panel: your name, email address, password, two-factor authentication and the devices you are signed in on."
nav:
  group: The Panel
  order: 3
---

# Your account

Your account is your sign-in on the panel: your name, email address, password, two-factor authentication and the devices you are signed in on.

Open the user menu and select **Account Details**. The page is called **My Account** and has two tabs, **Personal Information** and **Active Devices**.

The **Personal Information** tab has three parts. **Personal Information** holds your name, company, language and email address. **Password** has the **Change Password** button. **Security Settings** holds the **Two-Factor Authentication** switch and **New Device Detection Notification**. **Save Changes** is at the bottom.

## Changing your details

1. Open **My Account > Personal Information**.
2. Change **First Name**, **Last Name**, **Company Name** or **Language**. First and last name are required. **Language** is the language of the panel for you. **Email Address** is read-only here. To change it, use **Change Email Address**.
3. Select **Save Changes**. You see `Personal information details have been updated.`

   ![My Account with First Name, Last Name, Company Name, Language, the email and password buttons, the Two-Factor Authentication switch and Save](../../assets/single-server-edition/account-personal-information.webp)

**New Device Detection Notification** sets what counts as a new device when you sign in. Choose one of: `New IP address, device and browser`, `New IP address and device`, `New IP address and browser`, `New device and browser`, `New IP address`, `New browser` or `New device`. Save Changes keeps your choice.

Dates follow the panel default. You cannot set your own format: [Settings](settings.md). The user menu also has a **Dark Mode** switch: [Around the panel](around-the-panel.md).

## Changing your email address

1. In **Personal Information**, select **Change Email Address**.
2. Enter the new address and your current password.
3. Confirm. You see `Email address has been changed.`

If another account already uses the address, you see `The email has already been taken.`

## Changing your password

1. In **Password**, select **Change Password**.
2. Enter **Current Password**, **New Password** and **Confirm Password**.

A password needs at least 8 characters, with upper and lower case letters, numbers and symbols.

## Two-factor authentication

Two-factor authentication asks for a code from an app on your phone each time you sign in, in addition to your password.

**You can turn it on or off only within 15 minutes of signing in with your email and password.** A sign-in with **Sign in with PanelAlpha** does not count. If the time has passed, log out and sign in again with your password.

1. In **Security Settings**, select **Two-Factor Authentication**.
2. Scan the QR code with Google Authenticator or another authenticator app. You can also copy the code and enter it by hand.
3. Enter the code the app shows.
4. Save your backup codes and confirm that you did.

   :docs-video{src="../../assets/single-server-edition/account-two-factor-setup.webm" title="My Account, Security Settings: the Two-Factor Authentication switch opens the dialog with the QR code and the code to type by hand (both hidden here), the Code field, Confirm, then the backup codes (hidden here), the confirmation to tick and the button to go back to the account"}

You see `Two-Factor Authentication has been enabled.`

**Each backup code works once.** Keep them somewhere other than your phone. A backup code signs you in when you cannot use the app. The panel then gives you a new set and the old codes stop working. Save the new set the same way.

To turn two-factor authentication off, open the same dialog and confirm. You see `Two-Factor Authentication has been disabled.`

## Devices

**Active Devices** lists where you are signed in, with **IP Address**, **Browser**, **Device** and **Last Login**. Your current one is marked `Current Device`. The search field above the table finds a device by what it shows. Select the logout icon (tip **Log Out Device**) on a row to end that session. The bin icon (tip **Forget Device**) removes a device you no longer use. If there is nothing to show, the page says `No Active Devices Found`.

![My Account, Active Devices with each session, its IP address, browser, device and last login](../../assets/single-server-edition/account-active-devices.webp)

## Forgotten password

1. On the sign-in screen, choose the forgotten password link and enter your email address.
2. The panel shows `If an account exists for that e-mail address, we have sent it a link to reset the password.` whether or not the address has an account.
3. If the address has an account, an email arrives with the subject `Reset your password` and a **Reset Password** button. The link works for 60 minutes.
4. On the reset page, enter the new password twice. It follows the same rule as a password change.

The same steps, step by step: [Signing in](../getting-started/signing-in.md#if-you-forgot-your-password). If no email arrives, you can set a new password from the server: [On the server](../getting-started/on-the-server.md#resetting-your-password).

## Sign-in limits

After 5 failed sign-in attempts in a minute, or too many password-reset requests, the panel shows `Too Many Attempts.` Wait a minute and try again. How signing in works: [Signing in](../getting-started/signing-in.md).

If something does not work, see the messages you may meet on this page and what to do about them: [Troubleshooting: The panel and your account](../troubleshooting/panel-and-account.md#your-account).
