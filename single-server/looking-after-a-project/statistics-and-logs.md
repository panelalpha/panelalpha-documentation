---
title: "Statistics and logs"
description: "The panel shows who visits a project, how much disk and bandwidth it uses, and the logs it writes. Container projects also have a page for their containers."
nav:
  group: Looking After a Project
  order: 6
---

# Statistics and logs

The panel shows who visits a project, how much disk and bandwidth it uses, and the logs it writes. Container projects also have a page for their containers.

## Visitor statistics

Open the project and go to the **Monitoring** tab. **Visitors Chart** shows visits over the last week, month or year. Below it are cards for country, continent, region, referrer (the site a visitor came from), device operating system and browser.

The panel updates the statistics once a day. A new project shows `There is no data to display yet`. Check again the next day.

![The Monitoring tab of the Northwind Journal project: the Visitors Chart for Last Week, then Last Month, and the cards Visits By Country, Visits By Continent, Visits By Referrer, Visits By Device Operating System and Visits By Device Browser](../../assets/single-server/statistics-monitoring-tab.gif)

The **Project Details** tab also shows **Unique Visitors**, **Average Session Time** and **Bandwidth**. The session time is an estimate.

The country, continent and region cards stay empty until you download the visitor location data on the server: [On the server](../getting-started/on-the-server.md#visitor-countries).

## Usage and limits of a project

**Advanced > Project Details** shows how much RAM, CPU and disk the project uses now, and the limits it may use. A limit stops one project from taking the whole server.

<!-- TODO screenshot: assets/single-server/project-usage-limits.gif. Show Advanced > Project Details with the Project usage cards (RAM, CPU, Disk), then the Limits row in the Site details card, select Manage, change Memory (GB), open Advanced limits, select Save and show the Some limits apply after the next rebuild. note. -->

**Project usage** has three cards: **RAM**, **CPU** and **Disk**. Each card shows a ring with a percent, then **Used** and **Limit**. Above the cards, the panel says when the reading was taken, for example `Updated 2 minutes ago`.

- **A limit is set.** The percent is the share of that limit.
- **No limit on RAM or disk.** **Limit** shows `No limit`. The percent is the share of the whole server, and a **Server** line gives the size of the server.
- **No limit on CPU.** The percent is the share of one core, written as `% of 1 core`.
- **No recent reading.** The card shows a dash and `No recent data`. Look again in a few minutes.

The **Limits** row in the project details lists the limits that are set, for example `2 GB RAM · 1 core CPU · 10 GB disk`. `+2 more` means two other limits are set. A staging copy has no **Limits** row. Change the limits on the live project.

### Changing the limits

1. Open the project and go to **Advanced > Project Details**.
2. In the **Limits** row, select **Manage**.
3. In **Project limits**, change **Memory (GB)**, **CPU (cores)** or **Disk space (GB)**. Under a field, `Currently used` shows the present use. Under **Memory (GB)**, `Minimum` shows the lowest value.
4. To remove a limit, tick **No limit** under **CPU (cores)** or **Disk space (GB)**. **Memory (GB)** always needs a value.
5. To change the file and disk speed limits, select **Advanced limits**:
   - **Inodes (files)**: how many files and folders the project may hold.
   - **Disk read speed (MB/s)** and **Disk write speed (MB/s)**: how fast the project may read from and write to the disk.
   These three fields also have **No limit**.
6. Select **Save**. The panel shows `Project limits have been saved.`

The lowest memory is 256 MB for a container project and 6 MB for a PHP hosting project. CPU can be a fraction of a core, from 0.01. The other fields take whole numbers from 1.

**On a container project, only a new memory limit takes effect at once. Every other limit, and memory on a PHP hosting project, applies after the next rebuild.** After such a change the panel shows `Some limits apply after the next rebuild.` On a container project with a Git or archive source, **Redeploy now** starts the redeploy: [Git](../projects/git.md#redeploying). Select **Close** to leave it for later.

## Usage of the server

Open the user menu and select **My Server**. The page shows how much **RAM**, **Disk Space** and **CPU** the whole server uses. Below are graphs of:

- **CPU Usage**
- **Disk Throughput**: how much data the disk reads and writes.
- **Disk IOPS**: how many read and write operations the disk handles.
- **Network Traffic**: how much data goes in and out.
- **Network PPS**: how many network packets go in and out.

Choose **1 hour**, **12 hours** or **Live** in **Period**. **Live** is the last 5 minutes.

![My Server, Usage: RAM, CPU and Disk Space for the whole server, then the graphs Disk Throughput, Network Traffic and CPU Usage one after another](../../assets/single-server/statistics-server-usage.gif)

## Logs

Open the project and go to **Advanced > Logs**. Which tabs you see depends on the project:

- **Project Logs:** what was done to the project, with who did it and when.
- **Deploy Logs:** the deploy log of a container project. Pick a run from the list above the log.
- **WordPress Error Logs:** the error log of a WordPress site. It stays empty until WordPress is set to write errors to its log. Turn on **Debug Mode** in **Advanced > Developer Settings** for debugging. The page shows the last 100 entries. Use **Download** for the whole log.
- **PHP Logs:** the PHP errors. A container project has no PHP log here.
- **Webserver Logs:** the web server logs. Pick a file from the list at the top. The access log lists each visit. The error log lists the web server's errors.

Every tab except **Project Logs** has a **Download** button that saves the log you are looking at.

![Advanced > Logs of the Northwind Journal WordPress project: Project Logs, then PHP Logs with No PHP Logs Were Found, then Webserver Logs with the access log rows and the Download button](../../assets/single-server/statistics-logs-tabs.gif)

A tab with nothing to show says `No PHP Logs Were Found...` or similar. The web server log often holds more detail.

## Containers

A container project has the page **Advanced > Containers**. It lists each service of the project with its image, state, uptime and ports.

![Advanced > Containers of the ntfy project: the app service with its image, the state running, its uptime and port, the actions of the row, and the Actions menu at the top](../../assets/single-server/statistics-containers-page.gif)

- **Start All**, **Stop All**, **Restart** and **Pull & Redeploy** act on the whole project: [Projects](../projects/projects.md).
- Each service has its own **Start**, **Stop**, **Restart** and **Logs** actions. **Logs** shows the output of one service.

The panel asks for a confirmation before a restart, a stop or a redeploy.

### Public URLs

**Public URLs** is for container projects and developers. It forwards a domain of the project to a port of a service. The panel tells you if the domain is not one of this project or if the service already has a URL on that port.
