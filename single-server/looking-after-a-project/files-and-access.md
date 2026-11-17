---
title: "Files and access"
description: "You can work on a project's files in the file manager in the panel, or with an FTP or SFTP account from a program such as FileZilla. All three are in the..."
nav:
  group: Looking After a Project
  order: 4
---

# Files and access

You can work on a project's files in the file manager in the panel, or with an FTP or SFTP account from a program such as FileZilla. All three are in the project under **Advanced**.

## File manager

Open the project and go to **Advanced > File Manager**. Right-click a file or folder. The menu offers only the actions that fit: **Open** (folders), **View** (files the panel can preview, such as images), **Edit** (text files), **Download**, **Copy**, **Cut**, **Rename**, **Zip**, **Unzip** (archives), **Delete** and **Properties**. **Paste** shows after you copy or cut something. You can also upload files and create new folders and files.

<!-- TODO screenshot: assets/files-file-manager-menu.png. Show Advanced, File Manager with a file list and the right-click menu open on a file (Open, Edit, Download, Rename, Zip, Delete). -->

## FTP accounts

An FTP account gives a program access to a folder of the project.

1. Go to **Advanced > FTP Accounts** and select **Add Account**. The **Add FTP Account** window opens.
2. Enter the **Username** and choose the domain. The name takes letters and numbers only. Enter a **Password** (at least 8 characters). The **Directory** is optional.
3. When the server supports quotas, tick **Unlimited Quota** or enter a **Quota (MB)**.
4. Save.

**Directory** is a folder inside the project home. The field shows `/home/<name>/` before what you type, so enter the rest of the path, for example `/example.com/public_html`, and do not type the home again. The folder has to exist already; the Engine refuses anything else with `The directory must be an existing directory inside the project home.`

To change the password of an account, select **Edit FTP Account** on its row, enter the new **Password** (at least 8 characters, as for a new account) and select **Update**. The account keeps its username, directory and quota, and programs that use it need the new password from then on. If the panel refuses the new password, the window stays open and the reason is shown under the field.

To see the details to connect, open the login details of the account: **FTP Username**, **FTP Host** and **FTP Port**. The host is the main domain of the project, and the port is 21. On a project on the panel's free `panelalpha.online` address the host is the server's own IP address instead, because the PanelAlpha proxy that serves that address carries the website only, not port 21.

<!-- TODO screenshot: assets/files-ftp-login-details.png. Show the FTP login details with FTP Username, FTP Host (example.com) and FTP Port 21. No real passwords. -->

## SFTP accounts

An SFTP account gives a program access over an encrypted connection.

1. Go to **Advanced > SFTP Accounts** and select **Add Account**. The **Add SFTP Account** window opens.
2. Enter the **Username**. The panel puts a prefix before it. Use letters, numbers and underscores.
3. In **Authentication Method**, choose **Password** or **SSH Key**. For a password, you can have the panel generate one. **An SFTP password needs at least 8 characters with upper and lower case letters, a number and a symbol.** An FTP password needs only 8 characters. For a key, paste the **Public key**.
4. Save.

Keys can be `ssh-ed25519`, `ecdsa-sha2-nistp256`, `ecdsa-sha2-nistp384`, `ecdsa-sha2-nistp521`, a security key (`sk-ssh-ed25519@openssh.com` or `sk-ecdsa-sha2-nistp256@openssh.com`) or `ssh-rsa` from 2048 to 16384 bits. Paste one key on one line. The panel refuses any other key with `The public key must be one OpenSSH key line: ssh-ed25519, ecdsa-sha2-nistp256/384/521 or ssh-rsa of at least 2048 bits.`

The login details show **SFTP Username**, **SFTP Host** and **SFTP Port**. The host is the main domain of the project, or the server's own IP address on a project on the panel's free `panelalpha.online` address, which the PanelAlpha proxy serves on the web ports only. The port is 2222.

<!-- TODO screenshot: assets/files-sftp-login-details.png. Show the SFTP login details with SFTP Username, SFTP Host (example.com) and SFTP Port 2222, and the Actions menu with Disconnect All. No real passwords. -->

To cut off everyone at once, open **Actions** and select **Disconnect All**.

## Password for a whole site

The panel has no feature to protect a site with a password.

## Troubleshooting

**"The username field must not be greater than 32 characters."** On **SFTP Accounts**, the name you type may not pass 32 characters. The prefix the panel puts before it counts as well, so a name that fits 32 characters alone but whose prefix makes it longer is refused with **"The username is too long for this project."** Use a shorter name.

**"The user field must not be greater than 32 characters."** On **FTP Accounts**, the name may not pass 32 characters. Use a shorter name.

**"Username can only contain letters and numbers."** On **FTP Accounts**, an underscore, hyphen, dot or space in the name is refused. Use only the letters a to z, A to Z and the digits 0 to 9.

**"Invalid file name."** The name has characters the file manager does not accept. Use a plainer name.
