---
title: "Files and access"
description: "You can work on a project's files in the panel's file manager, or with an FTP or SFTP account from a program such as FileZilla. All three are under Advanced."
nav:
  group: Looking After a Project
  order: 4
---

# Files and access

You can work on a project's files in the file manager in the panel, or from a program such as FileZilla with an FTP or SFTP account. All three are in the project under **Advanced**.

An **FTP account** is a username and password that give a program access to a folder of the project. An **SFTP account** does the same over an encrypted connection.

## File manager

Open the project and go to **Advanced > File Manager**. It works like the file window on your own computer. On the left is the tree of folders. On the right is the list of what is inside the folder you opened. The list has the columns **Name**, **Size**, **Type** and **Date**. The bar at the bottom shows how many items you selected and their size.

The buttons at the top are:

- **Back**, **Forward** and **Up:** move between the folders you opened, or one level up.
- **Refresh:** read the folder again.
- **New file** and **New folder:** ask for a name and create it. A name cannot contain `\` or `/`.
- **Upload:** add files from your computer.
- **Delete**, **Copy**, **Cut** and **Paste:** act on the items you selected. Paste puts copied or cut items into the folder you opened.
- **Search:** find a name in the folder.
- The grid and list buttons switch how the items look. Another button shows or hides hidden files. The last one opens the File Manager in full screen.

Right-click a file or a folder to open its menu. The menu offers only what fits the item:

- **Open:** opens a folder.
- **View:** shows a preview of a file such as a picture.
- **Edit:** opens a text file in an editor, so you can change it and save it.
- **Play:** plays a sound or video file.
- **Download:** saves a file on your computer.
- **Copy** and **Cut:** put the item in the clipboard. Use **Paste** in another folder to finish. Cut moves the item. Copy keeps the original.
- **Rename:** asks for a new name.
- **Zip:** packs the items into one archive.
- **Unzip:** unpacks an archive.
- **Delete:** removes the item after you confirm.
- **Properties:** shows the path and the date the item was changed.

::docs-video{src="../../assets/single-server-edition/files-file-manager-menu.webm" title="Advanced > File Manager of Immich: the folders library, postgres, upload and backups and the files .env and docker-compose.yml, then the right-click menu of docker-compose.yml with Download, Copy, Cut, Rename, Zip, Delete and Properties, and the menu of the folder library with Open"}
::

## FTP accounts

**Advanced > FTP Accounts** lists the accounts of the project. The columns are **User**, **Directory** (the folder the account can reach), **Usage / Quota** (how much space it uses and how much it may use), **Updated** and **Actions**. The buttons under **Actions** show the login details, edit the account and delete it.

1. Select **Add Account**.
2. In **Username**, type the name. After the `@` choose the domain of the project. The login is the name, the `@` and the domain, for example `editor@immich-9b17.panelalpha.online`. The name takes letters and numbers only.
3. Enter a **Password** (at least 8 characters). The eye button shows or hides it. **Generate Password** makes a strong one for you.
4. Enter the **Directory** if you want to limit the account to one folder. The field is optional.
5. Keep **Unlimited Quota** ticked, or untick it and enter a **Quota (MB)**, the most space the account may use.
6. Select **Add Account**. Select **Cancel** to close the window without saving.

**Directory** is a folder inside the project home. The field shows `/home/<name>/` before what you type, so enter the rest of the path, for example `/upload`. The folder has to exist already.

**To change the password of an account**, select **Edit FTP Account** on its row, enter the new **Password** and select **Update**. Programs that use the account need the new password from then on.

**To see the details to connect**, select the eye button on the row. The window shows **FTP Username**, **FTP Host** and **FTP Port**. The host is the main domain of the project, and the port is 21. On a project on a `panelalpha.online` address, use the server's IP address as the host. The window does not show the password. Use the one you set.

::docs-video{src="../../assets/single-server-edition/files-ftp-login-details.webm" title="Adding an FTP account to Immich: Add Account, the name editor, a hidden password, Add Account again, a message that the FTP account has been added, then the eye button of the new row opens the login details with FTP Username, FTP Host and FTP Port"}
::

## SFTP accounts

**Advanced > SFTP Accounts** lists the accounts with the columns **User** and **Updated**, and the buttons to show the login details, edit and delete. The **Actions** menu at the top has **Disconnect All**.

1. Select **Add Account**.
2. Enter the **Username**. The panel puts the prefix of the project before it, for example `immich_`. Use letters, numbers and underscores.
3. In **Authentication Method**, choose **Password** or **SSH Key**.
   - **Password:** type one, or select **Generate Password**. **An SFTP password needs at least 8 characters with upper and lower case letters, a number and a symbol.**
   - **SSH Key:** paste the **Public key**, on one line. It is the public half of a key pair made on your computer. Use an ed25519 or RSA key (at least 2048 bits).
4. Select **Add Account**.

To see the details to connect, select the eye button on the row. The window shows **SFTP Username**, **SFTP Host** and **SFTP Port**. The host is the main domain of the project, and the port is 2222. On a `panelalpha.online` address, use the server's IP address as the host.

::docs-video{src="../../assets/single-server-edition/files-sftp-login-details.webm" title="The SFTP Accounts page of Immich: Add Account with the username upload after the prefix immich_, Authentication Method switched from Password to SSH Key, Cancel, and the eye button of immich_deploy opens the login details with SFTP Username, SFTP Host and SFTP Port"}
::

To cut off everyone at once, open **Actions** and select **Disconnect All**. The panel asks you to confirm.

The panel cannot password-protect a whole site.

Something not working? Messages and fixes: [Troubleshooting: Looking after a project](../troubleshooting/looking-after-a-project.md#files-and-access).
