---
title: Files, FTP and SFTP
description: Each project's files live in that account's home directory. For repository deploys, the application itself is in a subdirectory called project.
nav:
  group: Capabilities
  order: 6
---

# Files, FTP and SFTP

Each project's files live in that account's home directory. For repository deploys, the application itself is in a subdirectory called `project`.

**That subdirectory matters more than anything else on this page.** Files placed at the top level of the account are not served by the webserver. "I uploaded my site and nothing changed" is nearly always this.

## Through your assistant

```text
Upload this archive into the project and deploy it: /path/to/app.zip
```

```text
Write these files into the project and rebuild.
```

The assistant can upload a file, write files directly, download a file from an address or deploy a whole archive.

An archive goes through the deploy process. It is unpacked into `project`, identified and started, like a repository. Once the deploy has unpacked the archive, it removes that archive and only that one. Anything else in the folder the archive was staged in stays, including an archive staged there for a later deploy. The files the archive holds for that folder are written into it. All of this happens inside the deploy, so a finished deploy has nothing left to delete.

```text
Download https://example.com/plugin.zip into /project on this project.
```

The file is saved into a directory that already exists. Only an `http` or `https` address works. An address that points at the server itself or at a private network is refused. A file over the size limit (2 GB) is not kept, and nothing is left behind.

This does not deploy the file. On a site deployed from an archive or from files, say "and rebuild" if the running site should pick it up.

```text
List what is in /project on this project.
```

The assistant sees each entry's name, type, size, change time and permissions, folders first. Links are shown as links, with where they point.

It reads the folder as the project's own account. These are refused:

- a path that leads out of the account's home, including one that gets out through a link (the answer is 422 with the code `outside_home`)
- a path that is a file

A very long folder is cut off after 5000 entries by default, and the answer says so.

```text
Set the permissions on /project/script.sh to 755.
```

That changes the one file or directory. It does not go into folders inside it. The mode is three or four digits from 0 to 7, such as `755` or `0644`.

```text
Move everything directly inside /project/incoming into /project.
Leave files that are already there.
```

Only the immediate children move. The destination directory must already exist. If you do not say to leave existing files, a file with the same name is replaced.

**After changing files on a site deployed from an archive or from files, rebuild.** The running application does not pick up changed files on its own. It runs the version that was built. Say "and rebuild" as part of the request.

**A project connected to git is different.** A rebuild clones its branch again, so a file written into `/project` is gone afterwards. Commit the change to the repository and ask for a pull instead. A rebuild keeps the data the application writes in its volumes or a database. It also keeps what the application writes under container paths named in the project's `persist-paths` setting ([CLI commands](../commands/pae-cli.md)).

## FTP and SFTP

To let someone manage a project's files directly, ask your assistant. If it cannot create an account, see [Decide what the assistant may do](../connecting-your-ai/your-assistant.md#decide-what-the-assistant-may-do).

```text
Create an FTP account on this project for user ftpuser1 on shop.example.com,
with directory /project.
```

The directory must already exist inside the project's home. On a repository-deployed project it is `/project`. A folder that does not exist is refused. Leave the directory out to give the account the whole home.

On traditional PHP hosting each domain has its own folder, `/<domain>/public_html` (`/shop.example.com/public_html` here). The domain shows it as its `document_root`. There is no `/public_html` at the top.

An FTP account needs a password of at least 8 characters, and the engine does not generate one. Do not type it into the chat. The assistant sends a paste page for it, or makes one up and shows it to you. An SFTP account takes a password the same way, or a public key. You get credentials to hand over.

Prefer SFTP where the software at the other end supports it. Plain FTP sends the password unencrypted.

An SFTP login is the project's name, an underscore and a suffix (`tshop_sftp1`). You can give the suffix alone, and the engine adds the `tshop_` part. The whole login, with that part, is at most 32 characters. Each login is one account, and deleting it removes it completely.

An SFTP login can only move files. It cannot open a shell, and it sees only that project's files.

The project's plan limits set how many accounts it may have.

Deleting an SFTP account also ends the sessions it has open. To cut someone off without deleting the account, for example after a password leaked, ask the assistant to end the sessions:

```text
End every open SFTP session of the account tshop_sftp1 on this project.
```

To end the sessions of every SFTP account on the project at once, say "all SFTP sessions on this project". The answer says how many were open. People can log in again straight away unless you also change the password or delete the account.

**Upload into `/project/`**, not into `/`. Anything at the top level is outside what the webserver serves. On traditional PHP hosting (including most existing WordPress sites), upload into the domain's `/<domain>/public_html` instead. Those files are served as soon as you upload them. If you are unsure which applies, ask the assistant to inspect the project.

An upload keeps the permissions the file has on your computer. A file only you can read (mode `600`) is not served, and its address answers 403. Upload it as `644`, or run `chmod 644` on it over SFTP.

The name must be a plain file name, with no folder part. PHP keeps only the part after the last `/` or `\` of a multipart file name. The engine therefore refuses a name such as `sub\upload.txt` with `file_name_changed` and writes nothing. Otherwise it would replace another file with the shortened name.

## Troubleshooting

**"I uploaded my files and the site did not change."**
There are two likely causes, most likely first.

- The files went to the account's top level instead of into `project/` (or `<domain>/public_html` on traditional PHP hosting). Move them.
- The site was deployed from an archive or from files and needs a rebuild. The running application is still the version built earlier.

On a project connected to git, a rebuild replaces the uploaded files with the repository's, so the change has to go into the repository. Traditional PHP hosting, including WordPress in `<domain>/public_html`, needs no rebuild after an upload.

**"Permission denied" on upload.**
The account has hit its disk quota. A path outside the account's home directory is a different refusal: a 422 with the code `outside_home`. Ask the assistant to check the project's usage against its limits.

**"The file name ... would be stored as ..." on upload.**
The name has a folder part, for example `sub\upload.txt`. PHP keeps only the part after the last `/` or `\`. The engine refuses the name with the code `file_name_changed` and writes nothing, so the file with the shortened name is left as it was. Upload the file under a plain name.

**"... is not a folder the project's account can write to" on upload, write, new folder or unzip.**
The folder exists but does not belong to the account. Nothing was written, created or unpacked. The answer is a 422 on `path` with the code `not_writable`. On traditional PHP hosting the top level of the account (`/`) is one of these folders. Upload into the folder the site is served from instead: `/project`, or the domain's `/<domain>/public_html` on traditional PHP hosting.

**"Could not connect to ...", "... answered HTTP 404." or "... timed out." when fetching a URL.**
The engine could not download the address, so nothing was written. The answer is a 422 on `url` with the code `download_failed` and a `reason` (`resolve`, `connect`, `timeout`, `http` with `http_status`, `tls`, `interrupted`, `other`). Check that the address opens from the internet without a login, then fetch it again.

When the download worked but the file could not be saved where you asked, the answer is a 422 on `path` with `write_failed`. Pick a folder the project can write to.

**Unzip answers 409 `destination_exists`.**
The folder already holds a file or folder with the same name as one the archive unpacks to at its top level. Unpacking never replaces or merges, so nothing was extracted. The answer names the entries. Unpack into a new or empty folder, or move the existing entries away first.

**The FTP account connects but shows an empty folder.**
The account shows the directory it was set up with. Compare that with where the site's files are: `/project` for repository deploys, or the domain's `/<domain>/public_html` on traditional PHP hosting.

**I deployed an archive and the site is showing a directory listing.**
Your archive has a wrapper folder inside it, so the site's files ended up one level too deep. Repackage it with the files at the top level of the zip.

**Unpacking an archive fails.**
The answer is a 422 on `zip_path`. The code is `not_found` when no file is there. Otherwise the answer gives the reason the archive was refused or could not be unpacked.

Unzip reads a `.zip`, `.tar.gz`, `.tgz` or `.gz` by its name, in any letter case. It refuses any other type, such as `.rar` or `.7z`, with the reason.

The engine checks every entry in the archive before it unpacks. It refuses an archive that has:

- absolute paths
- paths that climb out of the folder
- links that point outside it
- zip entry names that unzip would change while unpacking (a control character, byte 0xff or a trailing `;1`)
- more than 100,000 entries, or more than 5 GB once unpacked

It unpacks as the project's own account, so it cannot write where that account cannot.

**Unpacking a zip made on Windows reports a warning about backslashes.**
Some Windows tools write backslashes between folder names.

- A zip that says it was made on Windows is unpacked into the right folders. The warning comes back in `warnings` (in the deploy log for an archive deploy). You have nothing more to do.
- A zip that uses backslashes without saying it was made on Windows is refused, because each backslash would end up inside a file name. Re-create it with forward slashes, for example with 7-Zip or `tar -a -cf archive.zip <folder>`.
- A zip made on VMS or an Amiga is refused too. Re-create it on Windows, macOS or Linux.

## More requests to try

**Mode** is the lowest permission mode that allows the request: read, modify or full ([Decide what the assistant may do](../connecting-your-ai/your-assistant.md#decide-what-the-assistant-may-do)). The examples assume a project named `tshop`.

**Look before touching**

```text
In tshop, list /project. Then tell me if /project/index.html exists, show its
size and permissions, and show me the first 200 bytes.
```

Mode: read, and modify for the last part. Tools: `file_list` with `show_hidden` and `limit`, `file_exists`, `file_stat`, `file_download` with `offset` and `length`. Check: the three answers agree, and the download returns only the range asked for. `file_download` can hand out files that hold passwords, so a read-only token does not get it. A path outside the home, or a file given to `file_list`, is refused.

**Write, upload, fetch**

```text
In tshop, create the folder /project/notes, write hello.txt into it with the
text "test run", upload a small file named upload.txt, and download
https://www.iana.org/robots.txt into the same folder.
```

Mode: modify. Tools: `file_mkdir` (with `parents`), `file_write`, `file_upload`, `file_fetch`. Check: all three files exist afterwards with the content asked for. A non-http address for the fetch is refused, and so is an address on the server itself or a private network.

**Move, copy, rename**

```text
In tshop, copy /project/notes/hello.txt to /project/notes/hello-copy.txt, rename
the copy to greeting.txt, and move everything directly inside /project/notes
into /project/archive. Keep files that are already there.
```

Mode: modify. Tools: `file_copy`, `file_move`, `file_move_contents` (`override` false), `file_mkdir`. Check: only the immediate children moved, the destination folder existed first, and an existing file with the same name was not replaced.

**Permissions**

```text
In tshop, set /project/notes/hello.txt to mode 640, then to 999.
```

Mode: modify. Tools: `file_chmod`, `file_stat`. Check: 640 is applied and read back. 999 is refused: a mode is three or four digits from 0 to 7.

**Archive and unpack**

```text
In tshop, zip /project/notes into /project/notes.zip, then unzip it into
/project/restored.
```

Mode: modify. Tools: `file_zip`, `file_unzip`, `file_exists`. Check: the zip exists, and the unpacked folder has the same files. An archive with an entry that climbs out of the folder is refused. Unpacking never replaces or merges into what is already there: if the folder already holds a file or folder of the same name as one the archive unpacks to at its top level (for a `.gz`, the unpacked file), the request is refused with 409 and nothing is extracted. An entry of that name that appears while the archive is being moved in, whether a file, a folder or a link, is refused with the same 409 and left as it is; entries moved in before it stay. A folder the project cannot write to is refused with 422. Unpack into a new or empty folder instead; a folder that does not exist yet is created.

**Delete**

```text
In tshop, delete /project/restored and everything in it.
```

Mode: full. Tools: `file_delete` with `recursive`. Check: the assistant asks first, and the folder is gone. Without `recursive`, a non-empty folder is not deleted.

**Change a file and have the site follow**

```text
In tshop, write "maintenance" into /project/public/status.txt and rebuild so
the running site picks it up.
```

Mode: modify. Tools: `file_write`, `project_rebuild`, `task_get`. Check: on a project deployed from an archive or from files, the rebuild is part of the answer and the file is served afterwards. On a project connected to git, the assistant says a rebuild clones the repository again and would delete the file, and suggests committing it instead. Traditional PHP hosting needs no rebuild.

**FTP**

```text
Give tshop an FTP account ftpuser1 for shop.example.com with directory /project,
a 50 MB quota and a strong password you generate. Then raise it to unlimited,
change its password, and delete it.
```

On traditional PHP hosting, ask for directory /shop.example.com/public_html instead: neither `/project` nor `/public_html` exists there, each domain's files are in `/<domain>/public_html`.

Mode: full. Tools: `ftp_account_create`, `ftp_account_list`, `ftp_account_update`, `ftp_account_delete`. Check: a password of at least 8 characters is sent, since the engine makes none up, the credentials come back once, and the quota reads back as 50 and then with `unlimited_quota: true`. An unlimited account's `quota` is not 0: it shows the project's disk space limit, or -1 when that is unlimited. The account is gone from the list at the end.

**SFTP with a password and with a key**

```text
Give tshop an SFTP account tshop_sftp1 with a password. Then change it to key
login using this public key:
ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIApWAkKVzgReKYul8SQpaYM0MJfdnc92MJi/XNWjUoKh example
Delete it afterwards.
```

Mode: full. Tools: `sftp_account_create`, `sftp_account_list`, `sftp_account_update` with `auth_method` set to `public_key` and the key as `public_key`, `sftp_account_delete`. Check: an SFTP account name is the project's name, an underscore and a suffix (`tshop_sftp1`), a suffix alone gets the prefix added, and a login over 32 characters is refused. After the update the account accepts the key and not the password. The key is one line of a `.pub` file: Ed25519, ECDSA, a security-key form of either, or RSA of at least 2048 bits. A shorter RSA key is refused as `ssh_key_too_weak`, a DSA key as `ssh_key_type_not_accepted`, and a line whose key data is not a key of its type as `ssh_key_corrupt`. Prefer SFTP in the answer over plain FTP.

**End SFTP sessions**

```text
End the open SFTP sessions of tshop_sftp1 on tshop. Then end every SFTP session on tshop.
```

Mode: modify. Tools: `sftp_session_kill`, `sftp_session_kill_all`. Check: each answer says how many sessions were open, and a connected client is dropped. The account still exists and can log in again.

**A command inside the project**

```text
Run `ls -la /project | head` inside the tshop container and show me the output.
```

Mode: modify. Tools: `ssh_run` with `cwd` and `timeout`. Check: pipes work, a non-zero exit is reported in the answer rather than as an error, the same request on traditional PHP hosting is refused, and a command is stopped when the caller that started it goes away, instead of running on in the container.

## From the server

Copying single files in and out: [CLI commands](../commands/pae-cli.md#files).
