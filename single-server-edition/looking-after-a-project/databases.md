---
title: "Databases"
description: "A project can have MySQL databases and database users. This page shows how to create them, give access and open phpMyAdmin."
nav:
  group: Looking After a Project
  order: 2
---

# Databases

A project can have MySQL databases and database users. A **database** holds the data of a site. A **database user** is a login that lets a program use a database.

You manage both in the project under **Advanced > MySQL Databases**, which has three tabs:

- **Databases:** the databases of the project. Each row has the **Database** name, its **Size**, when it was **Updated** and two buttons under **Actions**: edit (rename) and delete.
- **Users:** the database users. Each row has the **User** name, the **Assigned Databases** it may use and when it was **Updated**. Under **Actions** are four buttons: privileges, edit, access details (the eye) and delete.
- **PHPMyAdmin:** a button that opens phpMyAdmin, a web tool for the data inside a database.

Both lists have a search box, **Enter search term**. The **Users** tab never shows the password of an existing user.

## Creating a database

1. Open the project, go to **Advanced > MySQL Databases** and select the **Databases** tab.
2. Select **Add Database**.
3. Enter the name in **Database**. The panel shows the project's prefix before it, for example `immich_`. You only type the rest.
4. Select **Add Database**. Select **Cancel** to close the window without a new database.

A name starts with a letter, an underscore or a hyphen. It uses only letters, numbers, underscores and hyphens. The panel tells you if it is too long.

::docs-video{src="../../assets/single-server-edition/databases-list-tab.webm" title="Advanced > MySQL Databases of Immich with the tabs Databases, Users and PHPMyAdmin: Add Database with the name newsletter, and the new database immich_newsletter in the list with its Size and Updated columns"}
::

## Creating a database user

1. Go to **Advanced > MySQL Databases** and select the **Users** tab.
2. Select **Add User**.
3. Enter a **Username** and a **Password** (at least 8 and at most 64 characters, no spaces). The panel shows the project's prefix before the name. Select **Generate Password** to let the panel make a password for you. The eye button next to the password shows or hides it.
4. Select **Add User**. Write the password down. The panel does not show it again.

The name follows the same rules as a database name.

A new user can use no database until you give it one.

## Giving a user access to a database

1. On the **Users** tab, open the privileges action of the user. The dialog is called **Database Privileges: <user>**.
2. Choose the **Database**.
3. Set **Privileges** to **None**, **Read Only**, **Read & Write Only**, **Full** or **Custom**. With **Custom** you tick each privilege.

You can also select the database name in the user's row to change the privileges for that one database.

## Connection details

On the **Users** tab, select the eye action of the user (its tooltip reads **Show Access Details**). The **MySQL Access Details** window shows three things:

- **MySQL User:** the login name, with the project's prefix.
- **MySQL Host:** the address of the database server.
- **MySQL Port:** the port of the database server.

Use them exactly as shown. The host works from the project's own code, not from your computer. The window does not show a password.

A WordPress project also lists its database name, user, password and host under **Advanced > Project Details**, in the **Installation Details** card. The user and password are masked. Select **Show** to read one.

::docs-video{src="../../assets/single-server-edition/databases-access-details.webm" title="The Users tab of MySQL Databases in Immich with the columns User, Assigned Databases and Updated: the eye action of immich_user opens MySQL Access Details with MySQL User, MySQL Host and MySQL Port"}
::

## Renaming and deleting

- **Rename a database.** Use the edit action of the row, enter a **New name** and save. A WordPress project gets its `wp-config.php` updated to the new name.
- **Change a user's password or name.** Use the edit action of the user.
- **Delete a database or a user.** **Deleting a database removes its data, and a backup is the only way back. Make one first:** [Backups](backups.md). Then use the delete action and tick the confirmation.

## phpMyAdmin

phpMyAdmin is a web tool that lets you work on the records of a database directly. The panel logs you in for you.

1. Go to **Advanced > MySQL Databases** and select the **PHPMyAdmin** tab.
2. Read the warning: `This feature should be used by advanced users only.` Changes made in phpMyAdmin go straight into the database.
3. Select **Log In to PHPMyAdmin**. phpMyAdmin opens and you are already signed in.

::docs-video{src="../../assets/single-server-edition/databases-phpmyadmin-tab.webm" title="The PHPMyAdmin tab of MySQL Databases in Immich with the warning This feature should be used by advanced users only and the pointer on the Log In to PHPMyAdmin button"}
::

If the project's domain does not point at the server yet, the panel warns that phpMyAdmin may not open and offers a temporary address until DNS is set.

## Importing a database

The **Databases** tab has no import button. Import a SQL file through phpMyAdmin.

Something not working? Messages and fixes: [Troubleshooting: Looking after a project](../troubleshooting/looking-after-a-project.md#databases).
