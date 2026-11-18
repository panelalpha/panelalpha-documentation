---
title: "Create a project"
description: "You create a project from WordPress, from the PanelAlpha GO! app catalog, from a Git repository or from an archive of files. Each way has its own steps."
nav:
  group: Projects
  order: 2
---

# Create a project

You create a project from WordPress, from an app in the PanelAlpha GO! catalog, from a Git repository or from files. WordPress and PanelAlpha GO! are the easy paths. Git and Files are for people who have their own code. WordPress makes a PHP hosting project. The other three make a container project: [Projects](projects.md).

## Starting

1. Go to **My Projects** and select **Create First Project**. Later, use the create button on the same page.
2. Choose **WordPress**, **Git**, **Files** or **PanelAlpha GO!**.

   :docs-video{src="../../assets/single-server/create-project-choose-source.webm" title="Add New on My Projects opens Choose how to create your project with WordPress, Git and Files"}

You can also drop one archive on the project list. The panel starts the files path. A folder or another kind of file is refused before the project is created.

## Creating a WordPress site

1. Choose **WordPress**. If the panel asks, choose **Create New WordPress Project**.
2. Enter the domain. Either map your own domain in **Map your own domain**, or pick a name under **Use a free subdomain**.
3. Select **Create Project**.

   :docs-video{src="../../assets/single-server/create-project-wordpress-form.webm" title="Creating a WordPress site: WordPress, Create New WordPress Project, the domain shop.example.com under Map your own domain, Create Project, then Your site is now being created for you and Congratulations! Your site has been installed!"}

The panel installs the newest WordPress in the **document root** of the domain (the folder the domain serves) and shows `Your site is now being created for you!`. It can take a few minutes. You can leave the page, and a notification tells you when it is done: [Around the panel](../the-panel/around-the-panel.md#notifications).

When it finishes, the screen reads `Congratulations!` and `Your site has been installed!`. Select **Start Creating** to open the project, or **Back To Dashboard** to return to the project list.

The WordPress administrator is `admin`, with your account email and a random password. You do not need the password. **WP Admin** in the project's top bar opens the dashboard signed in for you: [Managing WordPress](wordpress.md).

If the domain still needs DNS records, the panel lists them after the install. Set them at your DNS provider: [Domains and SSL](../looking-after-a-project/domains-and-ssl.md).

To move an existing site here instead: [Importing a WordPress site](importing-wordpress.md).

## Creating a project from PanelAlpha GO!

**PanelAlpha GO!** is a catalog of open-source apps from [go.panelalpha.com](https://go.panelalpha.com). PanelAlpha deployed and tested each of them on PanelAlpha Engine, the hosting software the panel runs on. The panel deploys the app you pick on your server as a Git project.

**The panel must be connected to PanelAlpha Hub first.** Until it is, the **PanelAlpha GO!** option is greyed out and reads `Connect this panel to PanelAlpha Hub first to browse and install apps.` Select **Connect to PanelAlpha Hub** to open **Settings** and connect: [Settings](../the-panel/settings.md#panelalpha-hub). The catalog is free with a PanelAlpha Hub account.

1. Choose **PanelAlpha GO!**. The catalog opens with the heading **Choose an App**.
2. Find an app. Type in the search field (at least 2 characters) a name, a category or the app it replaces, for example `Notion`. You can also use the category list on the left.
3. Select **Install** on the app's card. To read more first, select **Details**.

::docs-video{src="../../assets/single-server/create-a-project-go-catalog-choose-app.webm" title="Choosing PanelAlpha GO! in Choose how to create your project, then Continue opens Choose an App, where the category list on the left (Monitoring) and the search field with uptime narrow the cards, a card shows Install and Details, and Details opens the Uptime Kuma page with its Install button"}
::

After **Install**, the panel creates a container project from the app's repository and shows its live log. It does not ask for a name, a domain, a branch or environment variables. The project is then an ordinary Git project: [Git](git.md).

::docs-video{src="../../assets/single-server/create-a-project-go-catalog-deploy.webm" title="Install on Uptime Kuma opens Your project is being created with the steps Preparing environment, Cloning the repository and Running stack and the live log, then the project list shows Uptime Kuma with its progress and Cancel Installation until it is running"}
::

If PanelAlpha GO! cannot be reached, the catalog says `The catalog is unavailable`. Your other ways to create a project still work. Select **Try Again** to load it again later.

## Creating a project from Git

1. Choose **Git**.
2. Enter the **Repository URL**.
3. For a private repository over HTTPS, turn on **Private repository** and paste the **Access token**.
4. Select **Continue**.

:docs-video{src="../../assets/single-server/create-project-git-form.webm" title="Creating a project from Git: the Repository URL https://github.com/mdn/beginner-html-site.git, the Private repository switch, Continue, the branch step, then the deploy log until Deploy finished successfully"}

The panel checks the repository. If it has several branches or an `.env.example` file, the **Environment variables** step opens. Choose the branch and adjust the values, or select **Skip** to keep the defaults. Then the deploy starts and the panel shows its live log.

### An SSH address

An SSH address (for example `git@github.com:example/app.git`) works differently. The panel creates a **deploy key**, an SSH key that lets the panel read the repository. You add it to the repository:

1. Enter the address and the branch (the default is `main`).
2. Select **Generate deploy key**. The project is created now.
3. Add the shown key to the repository as a read-only deploy key.
4. Tick **I added the deploy key to the repository** and select **Continue**.

Details on keys, tokens and branches: [Git](git.md).

## Creating a project from files

1. Choose **Files**.
2. Drop one `.zip` or `.tar.gz` archive of your app, or select it with **click to browse**.
3. Review the **Environment variables** step if it opens, then deploy.

:docs-video{src="../../assets/single-server/create-project-files-upload.webm" title="Creating a project from files: Files, the drop area with click to browse, the archive landing.zip added, then the deploy until it finishes"}

The archive can be up to 256 MB. A folder, a loose `.sql` file, a second file or a bigger archive is refused before the project is created. Pack your project, and its database dump if it has one, into one archive first.

### Replacing the files later

Select **Upload new version** on the **Source** card of **Project Details**. It takes a `.zip` or `.tar.gz` archive, up to 256 MB.

If you select **Cancel Deploy** while the first upload runs, or close the window, the project waits for its files. **Project Details** then says `Waiting for your files`. Upload the archive with **Upload new version**.

Something not working? Messages and fixes: [Troubleshooting: Projects](../troubleshooting/projects.md#create-a-project).
