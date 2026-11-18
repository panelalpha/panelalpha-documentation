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

Open the project and go to **Advanced > File Manager**. You can upload files and create new folders and files. Right-click a file or folder to open its menu, for example to edit, unzip, rename, copy or delete it.

:docs-video{src="../../assets/single-server/files-file-manager-menu.webm" title="Advanced > File Manager of the Northwind Journal project: the site folder and public_html opened, and the right-click menu of wp-config.php"}

## FTP accounts

1. Go to **Advanced > FTP Accounts** and select **Add Account**.
2. Enter the **Username** and choose the domain. The name takes letters and numbers only. Enter a **Password** (at least 8 characters). The **Directory** is optional.
3. Tick **Unlimited Quota** or enter a **Quota (MB)**.
4. Save.

**Directory** is a folder inside the project home. The field shows `/home/<name>/` before what you type, so enter the rest of the path, for example `/example.com/public_html`. The folder has to exist already.

**To change the password of an account**, select **Edit FTP Account** on its row, enter the new **Password** and select **Update**. Programs that use the account need the new password from then on.

**To see the details to connect**, open the login details of the account: **FTP Username**, **FTP Host** and **FTP Port**. The host is the main domain of the project, and the port is 21. On a project on a `panelalpha.online` address, use the server's IP address as the host.

:docs-video{src="../../assets/single-server/files-ftp-login-details.webm" title="Adding an FTP account named editor with a generated password, then its login details with FTP Username, FTP Host and FTP Port"}

## SFTP accounts

1. Go to **Advanced > SFTP Accounts** and select **Add Account**.
2. Enter the **Username**. The panel puts a prefix before it. Use letters, numbers and underscores.
3. In **Authentication Method**, choose **Password** or **SSH Key**.
   - **Password:** you can have the panel generate one. **An SFTP password needs at least 8 characters with upper and lower case letters, a number and a symbol.**
   - **SSH Key:** paste the **Public key**, on one line. Use an ed25519 or RSA key (at least 2048 bits).
4. Save.

The login details show **SFTP Username**, **SFTP Host** and **SFTP Port**. The host is the main domain of the project, and the port is 2222. On a `panelalpha.online` address, use the server's IP address as the host.

:docs-video{src="../../assets/single-server/files-sftp-login-details.webm" title="Adding an SFTP account named deploy with a generated password, its login details with SFTP Username, SFTP Host and SFTP Port, and the Actions menu of the page"}

To cut off everyone at once, open **Actions** and select **Disconnect All**.

The panel cannot password-protect a whole site.

Something not working? Messages and fixes: [Troubleshooting: Looking after a project](../troubleshooting/looking-after-a-project.md#files-and-access).
