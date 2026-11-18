---
title: "The panel and your account"
description: "Problems when you sign in, move the panel to another address, or change your account or the panel settings, with what each message means and what to do."
nav:
  group: Troubleshooting
  order: 1
---

# The panel and your account

Problems when you sign in, move the panel to another address, or change your account or the panel settings, with what each message means and what to do.

## First login

**"The panel must be opened at its configured address (APP_URL) to finish the setup."** You opened the panel at another address, such as its IP. Open it at the address the installer printed (this is what the message calls APP_URL), over `https://`.

**The wizard does not accept the password.** It is missing one of the password rules in step 3 of [the setup wizard](../getting-started/first-login.md#using-the-setup-wizard). Add what is missing.

**The wizard is gone and you see a sign-in page.** Someone finished the setup first. Sign in with that account.

**The browser warns about the certificate.** The panel address has a self-signed certificate, which is one the server made for itself and browsers do not trust. This happens when the address is a name under `panelalpha.direct` other than the server's own name, when PanelAlpha Engine (the hosting software the panel runs on) has no public address, or when it is a domain of your own that does not point at the server yet. Use a domain of your own that points here: [The panel address](../getting-started/panel-address.md).

More about this: [First login](../getting-started/first-login.md).

## The panel address

**"Enter an address such as https://panel.example.com, without a path, query or fragment."** The address has something after the domain, such as `/login`, `?a=1` or `#top`. Remove it.

**"The port must be a number from 1 to 65535."** or **"The panel is served over HTTPS only, so its address starts with https://."** Use a port from 1 to 65535, or no port (`https://panel.example.com` uses 443), and start the address with `https://`.

**"The SSL certificate could not be issued. Point the domain's DNS at this server and try again."** The domain does not point at the server yet, or port 80 is closed. Fix the DNS record, then save again. The engine sometimes gives its own reason instead, such as `Could not obtain a certificate for <domain>: DNS problem ...`.

**"Could not obtain a certificate for <domain>: <domain> is under panelalpha.direct, a zone every engine in the fleet issues under and shares one 50-certificate weekly limit on. Set shared_zone_issuance to obtain one anyway, or give the project a domain you own"** The new address is under `panelalpha.direct` and is not the server's own name. Those names share a limit of new certificates, so the engine does not request one and the panel keeps its old address. Use the server's own name or a domain you own ([Where the panel answers](../getting-started/panel-address.md#where-the-panel-answers)). To turn certificates for shared names on, see [On the server](../getting-started/on-the-server.md#certificates-for-shared-names).

**"The panel is being deployed or backed up. Save again once that has finished."** Wait for the task to end, then save again: [Tasks](../the-panel/tasks.md). The same message appears when you save a new Application URL in **Settings** while the panel is busy.

**"This Engine cannot move the panel to another address. Update the Engine, then try again."** The engine is older than 2.3.0: [Requirements](../getting-started/requirements.md).

**"The Engine could not register the domain. Try again later."** The engine refused the new domain. Wait a little and save again.

**"The panel does not know its own Engine project, so the domain cannot be registered."** The panel was installed without the value that tells it which engine project it is. You set this on the server: [On the server](../getting-started/on-the-server.md#when-the-panel-does-not-know-its-own-project). Then save the address again.

**"<host> is the server's own name, which only the server's operator can give to an app. Allow it once on the server with `pae system:public-routing:allow <project> --host-domain`."** The panel cannot use the server's own name until you allow it on the server: [On the server](../getting-started/on-the-server.md#giving-the-panel-the-servers-own-name). Then save again.

**"This server lets the panel answer on port 443 only: another port opens it in the server's firewall, which only the server's operator can allow. Keep 443, or allow it once on the server with `pae system:public-routing:allow <project> --ports`."** Keep port 443, or allow the other port on the server: [On the server](../getting-started/on-the-server.md#allowing-another-port-for-the-panel).

More about this: [The panel address](../getting-started/panel-address.md).

## Your account

**"These credentials do not match our records."** The email or the password is wrong. Check both and try again.

**"Sign in again with your password to change two-factor authentication."** More than 15 minutes have passed since you signed in with your password, or you signed in with **Sign in with PanelAlpha**. Log out, sign in with your email and password and try again.

**"This login link is not valid any more. Log in again from PanelAlpha."** The sign-in link from PanelAlpha was used already or is too old, or it came from a PanelAlpha account other than the one that connected the panel. Start again from the login page. If it keeps failing, sign in to PanelAlpha with the account that connected the panel, or disconnect and connect again with yours: [Settings](../the-panel/settings.md#signing-in-with-panelalpha).

**"The entered code is invalid."** or **"The entered backup code is invalid."** The two-factor code is wrong or expired, or the backup code was already used. Each backup code works once. Enter the current code from the app, or use a backup code you have not used.

**"Two-Factor Authentication is on, but your backup codes could not be loaded. Try again; if it keeps failing, turn Two-Factor Authentication off and on again to get a new set."** The panel could not read your backup codes. Follow the message to get a new set.

More about this: [Your account](../the-panel/your-account.md).

## Settings

**"Enter a number from 1 to 365"** **Backups to keep** is outside that range. Enter a whole number from 1 to 365.

**"Choose a directory inside /home/<name>, /opt/panelalpha or /var/www/html/storage. Backups stored anywhere else are lost when the Engine is updated."** The local folder is outside the three allowed places. Pick a folder inside one of them.

**"Enter the secret itself, not a vault reference."** A vault reference is a value that starts with `vault:`. Paste the real secret instead.

**"... Backup storage is not writable: ..."** The panel wrote a test file to the store and the store refused it. The text after the colon is the store's answer, for example a wrong key, a wrong bucket or no permission. Fix the details and save again.

**"The Engine did not create the backup store."** The engine refused the store. Check the details you entered and save again.

More about this: [Settings](../the-panel/settings.md).
