---
title: "Create a project"
description: "You create a project from WordPress, from the PanelAlpha GO! app catalog, from a Git repository or from an archive of files. Each way has its own steps."
nav:
  group: Projects
  order: 2
---

# Create a project

A project is one site or app on your server. You start every project in the same place and pick one of four ways to build it.

## Starting

1. Go to **My Projects** and select **Add New** in the top bar. On an empty list, select **Create First Project**.
2. The window **Choose how to create your project** opens. Select one of the four options, then select **Continue**.

   :docs-video{src="../../assets/single-server/create-project-choose-source.webm" title="Add New on My Projects opens Choose how to create your project, where WordPress, Git, Files and PanelAlpha GO! are selected one after another and Continue waits at the bottom"}

You can also drop one archive on the project list. The panel then starts the Files way for you. A folder or another kind of file is refused before the project is created.

## Which way to pick

Each option in the window has one line of text under its name.

- **WordPress.** The text says "Create a new site or migrate an existing one." Pick it when you want a WordPress site. You can start a new, empty site, or copy a site that already runs at another host. A WordPress site is a PHP hosting project: [Projects](projects.md).
- **Git.** The text says "Connect GitHub, GitLab, or Bitbucket and deploy to your server." Pick it when your app lives in a Git repository, which is a place online where developers keep the code of an app. The panel downloads the code and runs it. When you change the code later, you can deploy again with one click.
- **Files.** The text says "Upload files or ZIP archive and deploy it to your server." Pick it when you have the app as files on your computer and no repository. You pack the files into one archive and upload it.
- **PanelAlpha GO!** The text says "Browse apps from catalog." Pick it when you want a ready app, such as a shop, a photo gallery or a monitoring tool, and you do not have any code. You choose the app and the panel installs it. Nothing else to enter.

Git, Files and PanelAlpha GO! make a container project. A container project runs the app in its own box on the server, apart from your other projects.

If you are not sure, start with WordPress for a website and PanelAlpha GO! for everything else.

## Creating a WordPress site

1. Choose **WordPress** and select **Continue**.
2. The window **Add New Project** has two cards. Choose **Create New WordPress Project** ("Just a few quick steps to create a new WordPress project") and select **Continue**. The other card, **Import Existing WordPress Project**, copies a site from another host: [Importing a WordPress site](importing-wordpress.md).
3. In **Enter Your Domain**, say which address the site gets. Choose one of two cards:
   - **Map your own domain.** Type a domain you own in the field **Enter domain name**. The panel checks it. Use this when the site must live at your own address, for example `stories.blog`. You set up the domain at your DNS provider after the install.
   - **Use a free subdomain.** Type a name in the first field (it shows `mydomain` until you type), and pick the ending from the list next to it, for example `panelalpha.online`. Use this to get a working address at once, with no DNS to set up.
4. Select **Create Project**. **Maybe Later** closes the window and creates nothing.

   :docs-video{src="../../assets/single-server/create-project-wordpress-form.webm" title="Creating a WordPress site: Add New, WordPress, Create New WordPress Project, Use a free subdomain with the name stories, Create Project, then Your site is now being created for you and Congratulations! Your site has been installed!"}

The panel installs the newest WordPress and shows `Your site is now being created for you!`. It can take a few minutes. You can leave the page, and a notification tells you when it is done: [Around the panel](../the-panel/around-the-panel.md#notifications). The two buttons under the text are **Back To Dashboard** (back to the project list) and **Start Creating**.

When it finishes, the screen reads `Congratulations!` and `Your site has been installed!`. Select **Start Creating** to open the project, or **Back To Dashboard** to return to the project list.

The WordPress administrator is `admin`, with your account email and a random password. You do not need the password. **WP Admin** in the project's top bar opens the dashboard signed in for you: [Managing WordPress](wordpress.md).

If the domain still needs DNS records, the panel lists them after the install. Set them at your DNS provider: [Domains and SSL](../looking-after-a-project/domains-and-ssl.md).

## Creating a project from PanelAlpha GO!

**PanelAlpha GO!** is a catalog of open-source apps from [go.panelalpha.com](https://go.panelalpha.com). PanelAlpha deployed and tested each of them on PanelAlpha Engine, the hosting software the panel runs on. The panel deploys the app you pick on your server as a Git project.

**The panel must be connected to PanelAlpha Hub first.** Until it is, the **PanelAlpha GO!** option is greyed out and reads `Connect this panel to PanelAlpha Hub first to browse and install apps.` Select **Connect to PanelAlpha Hub** to open **Settings** and connect: [Settings](../the-panel/settings.md#panelalpha-hub). The catalog is free with a PanelAlpha Hub account.

1. Choose **PanelAlpha GO!** and select **Continue**. The catalog opens with the heading **Choose an App**.
2. Find an app. Type in the search field a name, a category or the app it replaces, for example `Notion`. Use at least 2 characters. You can also pick a category in the list on the left, for example **Monitoring**. A line above the cards counts the apps that match.
3. Each card shows the app name, a short line about it and a picture. Select **Install** on the card to install the app. To read more first, select **Details**. The details page tells what the app does and has its own **Install** button.

::docs-video{src="../../assets/single-server/create-a-project-go-catalog-choose-app.webm" title="Choosing PanelAlpha GO! in Choose how to create your project, then Continue opens Choose an App, where the category Monitoring and the search uptime narrow the cards, and Details on the Uptime Kuma card opens its page with the Install button"}
::

After **Install**, the panel creates a container project from the app's repository and shows `Your project is being created` with its live log. It does not ask for a name, a domain, a branch or environment variables. Select **Back To Dashboard** if you do not want to watch. The new project shows on the list with its progress and turns `live` when it runs. The project is then an ordinary Git project: [Git](git.md).

::docs-video{src="../../assets/single-server/create-a-project-go-catalog-deploy.webm" title="Install on the Uptime Kuma card opens Your project is being created with the live log, Back To Dashboard shows Uptime Kuma on the project list while it is created, and then the card turns live"}
::

If PanelAlpha GO! cannot be reached, the catalog says `The catalog is unavailable`. Your other ways to create a project still work. Select **Try Again** to load it again later.

## Creating a project from Git

You need the address of the repository, for example `https://github.com/bagisto/bagisto` for the Bagisto shop.

1. Choose **Git** and select **Continue**. The window **Deploy from Git** opens ("Connect a repository to create a project").
2. Enter the **Repository URL**. Use an HTTPS address, or an SSH address (see [An SSH address](#an-ssh-address)).
3. Leave **Private repository** off for a public repository. For a private repository over HTTPS, turn the switch on. A field **Access token** appears. Paste a personal access token from your Git provider. It lets the panel read the repository. The eye icon in the field shows or hides the token. A link under the field points to the documentation that tells how to create a token.
4. Select **Continue**.

   :docs-video{src="../../assets/single-server/create-project-git-form.webm" title="Creating a project from Git: the Repository URL https://github.com/bagisto/bagisto, the Private repository switch that shows the Access token field, Continue, the Environment variables step, then Your project is being created with the live log"}

The panel connects to the repository, finds its branches and looks for a file named `.env.example`. Then the **Environment variables** step opens. It is optional. This step has:

- **Branch.** The branch of the repository to deploy. A branch is one line of work in the repository. The list shows up when the repository has more than one. The panel picks the main one. If you change the branch after you edited values, the panel warns you that the edits are reset.
- **The variables.** One field for each line of `.env.example`, filled with the value from the file. These are the settings of the app, for example its name or its mode. Leave them as they are to use the defaults, or change what you need. An empty field keeps the default of the app or one the server makes up.
- **Skip.** Skips the step and keeps the defaults.
- **Continue.** Saves your values and starts the deploy.

If the repository has no `.env.example`, the step is only a choice of branch.

The deploy starts and the panel shows `Your project is being created` with its live log. The log goes through **Preparing environment**, **Cloning repository** and **Running stack**. When it ends, the screen reads `Congratulations!`. Select **Back To Dashboard** to see the project, or **Project Details** to open it.

### An SSH address

An SSH address (for example `git@github.com:bagisto/bagisto.git`) works differently. The panel creates a **deploy key**, an SSH key that lets the panel read the repository. You add it to the repository:

1. Enter the address and the branch (the default is `main`).
2. Select **Generate deploy key**. The project is created now.
3. Add the shown key to the repository as a read-only deploy key.
4. Tick **I added the deploy key to the repository** and select **Continue**.

Details on keys, tokens and branches: [Git](git.md).

## Creating a project from files

Pack your project into one `.zip` or `.tar.gz` archive first. If it has a database, put the dump in the same archive.

1. Choose **Files** and select **Continue**. The window **Upload your project** opens ("Upload one .zip or .tar.gz archive with your project files").
2. Drop the archive on the area **Drop your project archive here**, or select **click to browse** and pick it. The archive shows in the area. To take it away, remove it and add another one.
3. Read the list **What can I upload?** under the area. It names the three things that work: your project folder (as a compressed copy), a database dump (`.sql`, `.sql.gz`, `.sqlite` or `.db`) and a backup archive from your old host. The panel unpacks the archive for you.
4. Select **Continue**. If the archive has an `.env.example` file, the **Environment variables** step opens, the same as for Git. Then the deploy starts.

   :docs-video{src="../../assets/single-server/create-project-files-upload.webm" title="Creating a project from files: Add New, Files, the drop area with click to browse, the archive landing.zip added, Continue, then Your project is being created with the live log until Congratulations!"}

Keep the window open until the upload is done. After that the panel unpacks the files and runs the project. You see the same live log as for Git, with the step **Unpacking your files**.

The archive can be up to 256 MB. A folder, a loose `.sql` file, a second file or a bigger archive is refused before the project is created. The window then says why.

### Replacing the files later

Select **Upload new version** on the **Source** card of **Project Details**. It takes a `.zip` or `.tar.gz` archive, up to 256 MB.

If you select **Cancel Deploy** while the first upload runs, or close the window, the project waits for its files. **Project Details** then says `Waiting for your files`. Upload the archive with **Upload new version**.

Something not working? Messages and fixes: [Troubleshooting: Projects](../troubleshooting/projects.md#create-a-project).
