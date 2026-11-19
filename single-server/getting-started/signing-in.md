---
title: "Signing in"
description: "How to sign in to the panel with your email and password or with PanelAlpha Hub, what to do when you forget the password, the two-factor code, and how to sign out."
nav:
  group: Getting Started
  order: 4
---

# Signing in

You sign in on the login screen. Open the panel address, for example `https://panel.example.com`. The panel works over HTTPS only. Once the setup wizard is done, the screen is called `Welcome Back`. Before that, the wizard shows instead: [First login](first-login.md).

There are two ways to sign in. You can use the email and password of the account you made in the wizard. If the panel is connected to PanelAlpha Hub, you can also use your Hub account.

## Signing in with your email and password

The login screen asks for your email first and your password second.

1. Enter your email address in **Email Address** and select **Log In**.
2. The screen now reads `You are signing in as` followed by your address. Enter your password in **Password** and select **Log In**.

:docs-video{src="../../assets/single-server/signing-in-email-password.webm" title="The Welcome Back screen: the email you@example.com and Log In, then the password step with You are signing in as you@example.com, the Password field, Remember Me, Forgot password?, the Sign in with PanelAlpha button and Log In, which opens My Projects"}

On the password step you also see these.

- **Remember Me.** Tick it to stay signed in on this browser.
- **Forgot password?** Opens the page that sends you a reset link: [If you forgot your password](#if-you-forgot-your-password).
- **Back.** Goes back to the email step, so you can fix a mistyped address.
- **Sign in with PanelAlpha.** Shows only when the panel is connected to PanelAlpha Hub: [Signing in with PanelAlpha Hub](#signing-in-with-panelalpha-hub).

If the email or the password is wrong, the screen shows `These credentials do not match our records`. The panel asks for the password even when the email is not known to it, so a mistyped email only shows up after the password step. After 5 failed attempts in a minute you see `Too Many Attempts.` Wait a minute and try again: [Your account](../the-panel/your-account.md#sign-in-limits).

## Signing in with PanelAlpha Hub

If you connected the panel to PanelAlpha Hub, the login screen has a **Sign in with PanelAlpha** button under the form, after the word `or`. Select it. You go to PanelAlpha Hub, sign in there, and come back to the panel already signed in. The same works from the **Login** button next to your server in the Hub.

Only the Hub account that connected the panel can sign in this way. Signing in through the Hub skips the two-factor code. If the button is missing, the panel is not connected: [Settings](../the-panel/settings.md#panelalpha-hub).

If the Hub link is old or was already used, the screen shows `This login link is not valid any more. Log in again from PanelAlpha.` and a **Log In** button. Select it to go back to the login screen, then try again.

## The two-factor code

If you turned on two-factor authentication, a second screen called `Two-Factor Authentication` follows the password step: [Your account](../the-panel/your-account.md#two-factor-authentication).

1. Open your authenticator app and read the current code.
2. Enter it in **Enter your code** and select **Verify**. **Back** returns to the password step.

:docs-video{src="../../assets/single-server/signing-in-two-factor.webm" title="The password step, then the Two-Factor Authentication screen with the Enter your code field shown as dots, Back and Verify, and the My Projects page after Verify"}

The screen says `If you don't have access to your Two-Factor Authentication code, use your backup code here.` Enter a backup code in the same field. A backup code works once. After you use one, the panel makes a new set of backup codes and shows them. Save them, tick `I confirm that I have copied and saved the backup codes in a safe place.` and select **Continue**.

A wrong code shows `The entered code is invalid.`

## If you forgot your password

1. On the password step, select **Forgot password?**.
2. On the **Forgot password** page, enter your email address in **Email Address** and select **Send Password Reset Link**.
3. A message appears: `If an account exists for that e-mail address, we have sent it a link to reset the password.` It shows this whether or not the address has an account.
4. Open the email called `Reset your password` and select **Reset Password**. The link works for 60 minutes.
5. On the **Reset password** page, enter the new password in **Enter new password** and again in **Confirm Password**, then select **Reset Password**. The password follows the same rules as in the wizard.
6. A message reads `Your password has been reset.` and the panel takes you to the login screen. Sign in with the new password.

:docs-video{src="../../assets/single-server/signing-in-forgot-password.webm" title="The password step, Forgot password?, the Forgot password page with the email you@example.com, Send Password Reset Link and the message that a link was sent, and the link Sign In to go back"}

:docs-video{src="../../assets/single-server/signing-in-reset-password.webm" title="The Reset password page: Enter new password and Confirm Password shown as dots, Reset Password, then the login screen with the message Your password has been reset"}

**False alert? Sign In** at the bottom of the Forgot password page takes you back to the login screen when you remembered the password after all.

If no email arrives, you can set a new password on the server: [On the server](on-the-server.md#resetting-your-password).

## Signing out

1. Select your avatar in the top bar to open the user menu.
2. Select **Log Out**.

You come back to the login screen.

:docs-video{src="../../assets/single-server/signing-in-log-out.webm" title="My Projects, the user menu opened from the avatar, Log Out, and the login screen after it"}

To end the session of another browser, use **Active Devices**: [Your account](../the-panel/your-account.md#devices).

Something not working? [Troubleshooting: The panel and your account](../troubleshooting/panel-and-account.md#your-account).
