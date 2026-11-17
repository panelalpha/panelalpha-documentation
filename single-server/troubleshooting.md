---
title: "Troubleshooting"
description: "This page lists the problems you are most likely to meet in PanelAlpha Single Server Edition, with what each one means and what to do."
nav:
  group: Troubleshooting
  order: 1
---

# Troubleshooting

This page lists the problems you are most likely to meet in PanelAlpha Single Server Edition, with what each one means and what to do. Each answer ends with a link to the page that explains more.

## Signing in

**"These credentials do not match our records."** The email or the password is wrong. Check both and try again. More: [Your account](the-panel/your-account.md).

**"The entered code is invalid."** The two-factor code is wrong or expired. Enter the current code from the app, or use a backup code you have not used. Each backup code works once. More: [Your account](the-panel/your-account.md).

**"Too Many Attempts."** You asked for a password reset link more than 6 times in a minute. Wait a minute and ask again. More: [Your account](the-panel/your-account.md).

**No password reset email arrives.** The panel shows the same message for every address, so check that you typed the right one. If the address has an account, the email has the subject `Reset your password` and its link works for 60 minutes. When the server's mail is not set up, the message is refused on the way and the panel never learns that it did not arrive. The person who runs the server can set a new password from the server. More: [Your account](the-panel/your-account.md).

## First login

**"The panel must be opened at its configured address (APP_URL) to finish the setup."** You opened the panel at another address, such as its IP address. Open it at the domain you gave at install, over `https://`. More: [First login](getting-started/first-login.md).

**The setup wizard does not accept the password.** It is missing one of the rules. The password needs at least 8 characters, with 1 lowercase character, 1 uppercase character, 1 number and 1 symbol. More: [First login](getting-started/first-login.md).

**The sign-in page says the account could not be created.** The install refused the values, and the panel keeps the setup wizard closed. The deploy log names the rule that failed. More: [Installing the panel](getting-started/install.md#when-the-account-cannot-be-created).

**The browser warns about the certificate.** The panel address has a certificate the browser does not trust. This happens on a name under `panelalpha.direct` other than the server's own name, and on a domain of your own whose DNS record does not point at the server yet. Use a domain of your own that points at the server. More: [The panel address](getting-started/panel-address.md).

## The panel address

**"The panel is served over HTTPS only, so its address starts with https://."** Change `http://` to `https://`. More: [The panel address](getting-started/panel-address.md).

**"The SSL certificate could not be issued. Point the domain's DNS at this server and try again."** The domain does not point at the server yet, or port 80 is closed. Fix the DNS record, then save again. More: [The panel address](getting-started/panel-address.md).

**"The panel is being deployed or backed up. Save again once that has finished."** Wait for the task to end, then save again. More: [Tasks](the-panel/tasks.md).

## Creating a project

**"The repository was not found. Check the repository address and that the token can read it if the repository is private."** The address is wrong, or the repository is private and the token cannot read it. More: [Create a project](projects/create-a-project.md).

**"The repository refused this token. Check the token and that it can read this repository."** The token is wrong or has no read access. More: [Create a project](projects/create-a-project.md).

**"Domain is already in use."** A project on this server already has that domain. Choose another one. More: [Create a project](projects/create-a-project.md).

**"Installation failed."** The install did not finish. Select **Try Again**. **Cancel Installation** deletes the project with its files and databases instead. A Git or files project shows the reason in its deploy log. More: [Create a project](projects/create-a-project.md).

**"This project deploys from an uploaded archive, and that archive is deleted when its deploy ends. Upload the archive again to retry."** **Try Again** has no archive left to deploy. Upload the archive again with **Upload new version** on the **Source** card of **Project Details**. More: [Create a project](projects/create-a-project.md).

## Managing projects

**"Cannot remove project with existing staging project."** The project has a staging copy. Delete the staging copy, then delete the project. More: [Projects](projects/projects.md).

**"The server operator has protected this project, so it cannot be deleted, stopped or redeployed from the panel."** The person who runs the server marked the project as protected. Ask them to lift the protection. More: [Projects](projects/projects.md).

**"The project is busy with another operation. Please try again later."** A deploy, backup or other task is running on the project. Wait for it to finish and try again. More: [Tasks](the-panel/tasks.md).

## Staging and preview links

**"This project already has staging environment."** A project can have one staging copy. Use it, or delete it and create a new one. More: [Staging and preview](projects/staging-and-preview.md).

**"The project has no live parent to push to."** The project is not a staging copy. **Push To Live** works only from a staging copy. More: [Staging and preview](projects/staging-and-preview.md).

**A preview link stopped working.** A preview link works for 15 minutes. Create a new one with **Get Temporary URL**. More: [Staging and preview](projects/staging-and-preview.md).

## Domains and SSL

**"DNS propagation is in progress"** appears in place of **Change Domain**. The project's domain does not point at this server yet. Set the records that **Set Your Domain** shows at your domain provider and wait. This takes from 2 minutes to a few hours. More: [Domains and SSL](looking-after-a-project/domains-and-ssl.md).

**"Domain cannot start with 'www.'"** Enter `example.com`. The panel adds `www` for you. More: [Domains and SSL](looking-after-a-project/domains-and-ssl.md).

**"Cannot delete main domain"** The main domain has no delete icon. Make another domain the main one first, then remove this one. More: [Domains and SSL](looking-after-a-project/domains-and-ssl.md).

**"The SSL certificate installation for example.com has failed. Please contact support."** Check that the DNS records are right and try again. More: [Domains and SSL](looking-after-a-project/domains-and-ssl.md).

**"Invalid certificate/key"** The certificate and the private key do not belong together. Check that you pasted the key of this certificate. More: [Domains and SSL](looking-after-a-project/domains-and-ssl.md).

## Backups

**"Choose files or databases to back up."** Both parts are switched off. Switch at least one part on. More: [Backups](looking-after-a-project/backups.md).

**"Cannot delete a backup that is being created."** Wait until the backup has finished. More: [Backups](looking-after-a-project/backups.md).

**"Backups are not available until the Engine has a backup container."** No backup store is working yet. Set one up in **Settings > Backups**. More: [Settings](the-panel/settings.md).

**"... Backup storage is not writable: ..."** The panel wrote a test file to the store and the store refused it. The text after the colon is the answer of the store, for example a wrong key, a wrong bucket or no permission. Fix the details and save again. More: [Settings](the-panel/settings.md).

## Git deploys

**"A deploy of this project is already running."** Wait for the running deploy to end, or cancel it in its Deploy Logs window. More: [Git](projects/git.md).

**"Could not register a webhook with the Git provider. Check that your token can manage webhooks for this repository."** The token cannot manage webhooks. Make a token with that permission and save it, then turn **Auto-deploy on push** on again. More: [Git](projects/git.md).

**The "Local changes detected" dialog opens when you pull or change the branch.** The site has files that its repository does not have. **Discard local changes** and **Force pull (overwrite changes)** delete those files, and you cannot recover them. **Push local changes first, then pull** keeps them. More: [Git](projects/git.md).

**"The repository has newer commits. Pull those changes before pushing from this site."** Select **Pull** first, then push again. More: [Git](projects/git.md).

## WordPress

**"This project is not a WordPress installation."** The project is a container project. WordPress screens work only on a WordPress site. More: [Managing WordPress](projects/wordpress.md).

**"Using your WordPress admin credentials will not work (probably due to Cloudflare/firewall/WAF restrictions). Please install the plugin manually."** A firewall on the old site blocks the login. Install the migration plugin manually. More: [Importing a WordPress site](projects/importing-wordpress.md).

**"Migration plugin not found in your WordPress."** The plugin is not installed or not active on the old site. Install and activate it, then try again. More: [Importing a WordPress site](projects/importing-wordpress.md).

## Files and FTP

**"Username can only contain letters and numbers."** On **FTP Accounts**, an underscore, hyphen, dot or space in the name is refused. Use only the letters a to z, A to Z and the digits 0 to 9. More: [Files and access](looking-after-a-project/files-and-access.md).

**"The directory must be an existing directory inside the project home."** The folder in **Directory** has to exist already. The field shows `/home/<name>/` before what you type, so enter only the rest of the path. More: [Files and access](looking-after-a-project/files-and-access.md).

## Cron jobs

**A cron job seems not to run.** The panel does not show what the job printed. Make the command write a file in the site's folder, wait for the schedule and open the file in **Advanced > File Manager**. If the file is missing, check that the command writes the whole path: the home directory is not writable and a relative path starts there. Also check the schedule and, on a container project, that the chosen **Service** is running. More: [Cron jobs and PHP](looking-after-a-project/cron-and-php.md).
