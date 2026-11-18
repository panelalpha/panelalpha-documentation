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

## Changing your details

1. Open **My Account > Personal Information**.
2. Change **First Name**, **Last Name**, **Company Name** or **Language**. First and last name are required, up to 20 characters each.
3. Save. You see `Personal information details have been updated.`

   ![My Account opened with Account Details in the user menu: First Name, Last Name, Company Name, Language and Change Email Address](../../assets/single-server/account-personal-information.gif)

**New Device Detection Notification** lets you choose what counts as a new device: a new IP address, device, browser or a combination. The panel stores the choice, but it does not send an email about a new device.

Dates follow the panel default. You cannot set your own format: [Settings](settings.md). The user menu also has a **Dark Mode** switch: [Around the panel](around-the-panel.md).

## Changing your email address

1. In **Personal Information**, select **Change Email Address**.
2. Enter the new address and your current password.
3. Confirm. You see `Email address has been changed.`

If another account already uses the address, you see `The email has already been taken.`

## Changing your password

1. In **Password**, select **Change Password**.
2. Enter **Current Password**, **New Password** and **Confirm Password**.

A password needs at least 8 characters, with upper and lower case letters, numbers and symbols. If the two new entries differ, you see `Passwords do not match.`

## Two-factor authentication

Two-factor authentication asks for a code from an app on your phone each time you sign in, in addition to your password.

1. In **Security Settings**, select **Two-Factor Authentication**.
2. Scan the QR code with Google Authenticator or another authenticator app. You can also copy the code and enter it by hand.
3. Enter the code the app shows.
4. Save your backup codes and confirm that you did.

   ![My Account, Security Settings: the Two-Factor Authentication switch opens the dialog with the QR code, the code to type by hand and the Code field (QR code and code hidden here)](../../assets/single-server/account-two-factor-setup.gif)

You see `Two-Factor Authentication has been enabled.`

**Each backup code works once.** Keep them somewhere other than your phone. A backup code signs you in when you cannot use the app. The panel then gives you a new set and the old codes stop working. Save the new set the same way.

To turn two-factor authentication off, open the same dialog and confirm. You see `Two-Factor Authentication has been disabled.`

## Devices

**Active Devices** lists where you are signed in, with **IP Address**, **Browser**, **Device** and **Last Login**. Your current one is marked `Current Device`. Select the logout icon (tip **Log Out Device**) on a row to end that session. The bin icon (tip **Forget Device**) removes a device you no longer use. If there is nothing to show, the page says `No Active Devices Found`.

![My Account, Active Devices: each session with its IP address, browser, device and last login, and the row marked Current Device](../../assets/single-server/account-active-devices.gif)

## Forgotten password

1. On the sign-in screen, choose the forgotten password link and enter your email address.
2. The panel shows `If an account exists for that e-mail address, we have sent it a link to reset the password.` whether or not the address has an account.
3. If the address has an account, an email arrives with the subject `Reset your password` and a **Reset Password** button. The link works for 60 minutes.
4. On the reset page, enter the new password twice. It follows the same rule as a password change: at least 8 characters, with upper and lower case letters, numbers and symbols.

This email is the only message the panel sends.

If you ask for a link twice within 60 seconds, the second request shows the same message and sends nothing. One IP address can ask at most 6 times a minute. The seventh request sends nothing and shows `Too Many Attempts.` Wait a minute and ask again.

### If no email arrives

The panel only hands the message over. The Engine's outgoing mail settings decide what happens next. They set the server the mail goes through, and the sender and DNS records that receiving servers check. If the relay is not set up, or the domain lacks those records, the message is refused on the way. The panel never learns that it did not arrive.

If no mail arrives, set a new password on the server. Run this as `root`, with your panel project name, your email and a new password that follows the rules above:

```bash
docker exec -i panel docker exec -i -u www-data panelalpha-v1-panel php /app/backend/artisan users:update you@example.com --password='<new password>'
```

It prints `User updated: you@example.com (password=***)`. The same command with `--email=` changes the address.

### More detail

- **The link.** It goes to the panel's Application URL.
- **How the panel sends mail.** A new install sends its mail through the server's relay. The Engine hands the panel the address of the mail server on its host, and the panel sends each message there. The message comes from `no-reply@` at the panel's own address, with the server name from [Settings](settings.md) as the sender name.
- **No relay address.** When the Engine gives no address, the panel writes each message to its log and sends nothing.

## Sign-in limits

Sign-in has these limits. When you reach one, wait a minute.

- **Sign-in:** 5 attempts a minute for each email address and IP address together. An attempt is one email address and password submitted together.
- **Two-factor code:** 5 attempts a minute for each sign-in.
- **Email screen:** the screen that only asks for your email address is counted apart, with 20 a minute from one IP address.

If something does not work, see the messages you may meet on this page and what to do about them: [Troubleshooting: The panel and your account](../troubleshooting/panel-and-account.md#your-account).
