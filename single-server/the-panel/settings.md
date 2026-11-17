---
title: "Settings"
description: "Settings holds the panel's own options: its name, language, date format, address, default domain for new projects and default backup schedule."
nav:
  group: The Panel
  order: 1
---

# Settings

Settings holds the panel's own options: its name, language, date format, address, the default domain for new projects and the default backup schedule.

Open it from the user menu with **Settings**. It has two tabs, **General** and **Backups**. Save a change with **Save Changes**. The button stays off until something changed. Afterwards you see `Settings saved.`

## PanelAlpha Hub

The card above the tabs connects this server to your PanelAlpha Hub account at [hub.panelalpha.com](https://hub.panelalpha.com). It reads `Free and optional. This server keeps working on its own either way.`

A connected server gives you:

- **App monitoring**: the status of every site on this server, in the Hub.
- **Reusable integrations**: link a service once, and every server of yours uses it.
- **One login**: one PanelAlpha account for all your servers, and **Sign in with PanelAlpha** on the panel's login page.

To connect:

1. Select **Connect to PanelAlpha**.
2. Sign in on PanelAlpha and approve. You come straight back here.
3. The card shows `This server is connected to PanelAlpha` and `Linked to the account <account>`.

If the card shows `PanelAlpha has not confirmed the connection yet.`, select **Check again**. If you wait too long, it shows `The link expired. Start again.` A connect link lasts 15 minutes by default.

**Signing in through PanelAlpha skips the panel's two-factor check.** The Hub sign-in counts as that check.

**Disconnecting removes this server from your PanelAlpha account and turns off Sign in with PanelAlpha until you connect again.** To disconnect, select **Disconnect** and confirm. If you signed up through the Hub and have no password, the panel asks you to set one in **My Account** first.

### What the panel reports to the Hub

While connected, the panel reports to the Hub every five minutes: the name, type and status of each project, the panel's version and its address. It also reports right after pairing and right after you save a new Application URL.

## General

- **Server name**: the name the panel shows in page titles. Required, up to 255 characters. The default is `My Server`.
- **Default language**: the language people see before they choose their own. The default is English.
- **Default date format**: how the panel writes dates. Choose `YYYY-MM-DD` (default), `DD.MM.YYYY`, `DD/MM/YYYY` or `MM/DD/YYYY`. Accounts have no date format of their own, so this one applies to everyone.

![Settings opened from the user menu, General tab: Server name, Default language, Default date format, Application URL, Default domain and Save Changes](../../assets/single-server/settings-general-tab.gif)

## Application URL

The **Application URL** is the address the panel uses in the links it sends, such as password-reset emails. Saving a new address also moves the panel to it.

**The address the panel has now stops answering when you save.** Point the new domain's DNS at this server first: [The panel address](../getting-started/panel-address.md).

## New projects

**Default domain** is the domain that new projects are named under. To set it:

1. Create a DNS record `*.example.com` that points at this server.
2. Open **Settings > General**, enter `example.com` in **Default domain** and select **Save Changes**.

A new project called `shop` then gets `shop.example.com`. Existing projects keep their domain.

Leave the field empty (`Not set`) and the Engine picks a name for each new project. Select the help icon next to the label to see how the Engine names new projects on your server and what it falls back to. The domain can have up to 253 characters.

## Backups

The **Backups** tab holds two things: the backup store and the schedule that new projects start with. A **backup store** is the place where backups are kept.

You do not have to set anything up. **A server with no store gets one.** The Engine's install creates a local store named `local` in `/opt/panelalpha/backups/local`. The panel backs up into it straight away. **Create Backup** works, and the schedule below applies to new projects.

**A server that already had a store keeps it.** The panel uses that store with nothing to set up.

Select **Change store** only if you want your backups somewhere else. The panel then uses your store.

The tab shows the store in use as its type and directory. The row beside **Change store** reads `Local · /opt/panelalpha/backups/local` for the `local` store above.

### Setting up a store

1. Open **Settings > Backups**.
2. Select **Configure store** (or **Change store** if one exists).
3. Choose a **Store type**: Local, S3, SFTP, FTP or FTPS.
4. Fill in the fields for that type and save. You see `Backup store saved.`

   ![Settings, Backups tab: Change store opens the store form with the Store type list (Local, S3, SFTP, FTP and FTPS) and the Directory of the Local store](../../assets/single-server/settings-backup-store-form.gif)

| Type | Fields |
|---|---|
| Local | **Directory** (default `/opt/panelalpha/backups/local`) |
| S3 | **Bucket**, **Access key ID**, **Secret access key**, **Region**, **Endpoint (optional)**, **Use path-style endpoint** |
| FTP and FTPS | **Host**, **Port**, **Username**, **Password**, **Remote path** |
| SFTP | **Host**, **Port**, **Username**, **Password** or **Private key (instead of a password)**, **Key passphrase (optional)**, **Remote path** |

**A local store must be inside `/home/<name>`, `/opt/panelalpha` or `/var/www/html/storage`.** A backup anywhere else is lost when the Engine is updated.

The panel tests the store before it saves it. A changed remote store becomes a new store. A local store at the same directory is reused. What happens to the store you leave is under [Going back to the server's store](#going-back-to-the-servers-store).

### Going back to the server's store

Select **Remove store** next to the store to stop choosing one. The schedule below and every backup taken from then on use the server's own store again, the one the tab showed before you chose yours. Projects keep the schedule they have. The dialog asks you to tick what happens to this store before **Remove store** becomes available.

What happens to the store you remove depends on who made it:

- **A store the panel created is deleted from the server** when nothing uses it. If a backup or a project's schedule still points at it, the Engine keeps it and the panel tells you.
- **A store the panel did not create** (one made with `pae backup:container:create` or by another panel) is never deleted. The panel only stops using it.

**Change store** treats the store you leave the same way: one the panel created is deleted when nothing uses it, and kept when a backup or a schedule still does.

On a server with no other store the page goes back to `No backup store configured`. The schedule fields are off until you set one up again.

### Schedule for new projects

These defaults apply to projects you create from now on, in this panel. Existing projects keep their own schedule, and so does a project another panel or the server's operator created.

- **Automatic backups**: Enabled (default) or Disabled.
- **Backups to keep**: a number from 1 to 365. The default is 7.
- **Frequency**: Daily (default), Weekly or Monthly. Weekly adds **Day of the week**. Monthly adds **Day of the month**, 1 to 28.
- **Time**: the hour, 0 to 23, in which the projects' backups start. The default hour is 03:00 UTC. You see the time in your local time.

Each new project gets its own minute inside that hour, so the projects of a server do not all back up at the same moment. The project's **Backups** tab shows its exact schedule. A project without a schedule starts its **Automatic Backups** dialog at this hour and its own minute too.

![Settings, Backups tab: the defaults for new projects, Automatic backups, Backups to keep, Frequency and Time](../../assets/single-server/settings-backup-schedule.gif)

For taking and restoring a backup of one project: [Backups](../looking-after-a-project/backups.md).

### More detail

- **Older servers that already had a store.** A store made with `pae backup:container:create` or by an earlier panel (a local one such as `panel-local-…` in `/opt/panelalpha/backups`) is kept when the Engine is installed or updated. No second store is added, no `local` folder is created and no backup moves. The row beside **Change store** reads `Local · /opt/panelalpha/backups` for such a store.
- **A server without a store.** If the Engine never created that store (it predates it and has not been updated since), every schedule field is off and the page says `Set up a backup store above to schedule backups for new projects.` Set one up as above.
- **A kept store comes back.** A store the panel created that was kept because a backup or schedule still used it is not forgotten. Choose the same directory again once that backup or schedule is gone. The panel shows it as its own store again, so **Remove store** then deletes it from the server.

## Troubleshooting

**"Enter a number from 1 to 365"**. **Backups to keep** is outside that range. Enter a whole number in it.

**"Choose a directory inside /home/<name>, /opt/panelalpha or /var/www/html/storage. Backups stored anywhere else are lost when the Engine is updated."** The local directory is outside the three allowed places. Pick a folder inside one of them.

**"Enter the secret itself, not a vault reference."** A secret field starts with `vault:`. Paste the real secret.

**"... Backup storage is not writable: ..."** The panel wrote a test file to the store and the store refused it. The text after the colon is the answer of the store, for example a wrong key, a wrong bucket or no permission. Fix the details and save again.

**"The Engine did not create the backup store."** The Engine refused the store. Check the details you entered and save again.

**"The panel is being deployed or backed up. Save again once that has finished."** You saved a new Application URL while the panel project was busy. Wait and save again.
