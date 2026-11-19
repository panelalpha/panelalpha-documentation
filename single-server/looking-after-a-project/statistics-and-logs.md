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

Open the project and go to the **Monitoring** tab. At the top, **Visitors Chart** draws the visits per day. Choose **Last Week**, **Last Month** or **Last Year** to change the span. The big number above the chart is the total of visits in that span. The small arrow beside it shows the change against the span before.

Below the chart is one card for each question. Each card lists the biggest groups first, with their share:

- **Visits Chart** (pages): which pages of the site visitors open most.
- **Visits By Country**, **Visits By Continent** and **Visits By Region:** where visitors are.
- **Visits By Referrer:** the site a visitor came from. `direct` means the visitor typed the address or used a bookmark.
- **Visits By Device:** the kind of device, for example a phone or a computer.
- **Visits By Device Brand:** the make of the device.
- **Visits By Device Operating System:** the system on the device, for example Windows or Android.
- **Visits By Device Browser:** the program the visitor used to open the site.

The panel updates the statistics once a day. A new project shows `There is no data to display yet`. Check again the next day.

::docs-video{src="../../assets/single-server/statistics-monitoring-tab.webm" title="The Monitoring tab of Immich: the Visitors Chart, then the cards below it scroll past, from the pages and referrers to the device, operating system and browser"}
::

The **Project Details** tab also shows **Unique Visitors**, **Average Session Time** and **Bandwidth**. The session time is an estimate.

The country, continent and region cards stay empty until you download the visitor location data on the server: [On the server](../getting-started/on-the-server.md#visitor-countries).

## Usage and limits of a project

**Advanced > Project Details** shows how much RAM, CPU and disk the project uses now, and the limits it may use. A limit stops one project from taking the whole server.

::docs-video{src="../../assets/single-server/statistics-and-logs-usage-limits.webm" title="Advanced opens Project Details of Immich with the Project usage cards RAM, CPU and Disk, the Manage link in the Limits row opens Project limits, Advanced limits shows Inodes, Disk read speed and Disk write speed, Memory (GB) changes from 2 to 4, and Save closes the window"}
::

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

::docs-video{src="../../assets/single-server/statistics-server-usage.webm" title="My Server, Usage: RAM, CPU and Disk Space for the whole server, then the graphs Disk Throughput, Network Traffic and CPU Usage one after another"}
::

## Logs

Open the project and go to **Advanced > Logs**. Which tabs you see depends on the project. A WordPress project like Stories has these four tabs, and a container project like Immich has **Project Logs** and **Deploy Logs**:

- **Project Logs:** what was done to the project, with who did it and when. Each row has the person with their role, the action and how long ago it happened. For example, `Initiated Backup Creation` is a backup you started. The list has a search box and a page selector.
- **Deploy Logs:** the deploy log of a container project. Pick a run from the list above the log. Each run shows its steps, such as cloning the repository, building the images and starting the app. A failed run shows where it stopped.
- **WordPress Error Logs:** the error log of a WordPress site. It stays empty until WordPress is set to write errors to its log. Turn on **Debug Mode** in **Advanced > Developer Settings** for debugging. The page shows the last 100 entries. Use **Download** for the whole log.
- **PHP Logs:** the errors of PHP, the language WordPress runs on. Each row has the **Time**, the **Level** (such as Warning) and the **Message**. A container project has no PHP log here.
- **Webserver Logs:** the logs of the web server, the program that sends your pages to visitors. Pick a file from the list at the top. The **access.log** file lists each visit with the **Time**, the **IP Address** of the visitor, the **Request**, the **Status** and the **Size**. A status that starts with 2 is fine, 3 is a redirect, 4 is a missing page or a refused request and 5 is a server error. The **error.log** file lists the web server's errors.

Every tab except **Project Logs** has a **Download** button that saves the log you are looking at.

::docs-video{src="../../assets/single-server/statistics-logs-tabs.webm" title="Advanced > Logs of the Stories WordPress project: Project Logs, then WordPress Error Logs, PHP Logs and Webserver Logs with the access.log rows and the Download button"}
::

A tab with nothing to show says `No PHP Logs Were Found...` or similar. The web server log often holds more detail.

## Containers

A container project has the page **Advanced > Containers**. It lists each service of the project. A service is one program of the app. Immich, for example, has `immich-server`, `immich-machine-learning`, `redis` and `database`. The columns are:

- **Service:** the name of the service.
- **Image:** the package the service runs from.
- **State:** `running` when it works.
- **Uptime:** how long it has been running.
- **Ports:** the ports the service listens on.

At the end of each row are four buttons. The first one stops the service, or starts it when it is stopped. The second restarts it. The globe opens **Public URLs** for that service, explained below. The last one shows the log of that service.

The **Actions** menu at the top acts on the whole project:

- **Start All:** starts every service.
- **Restart:** restarts every service.
- **Stop All:** stops every service.
- **Pull & Redeploy:** downloads the newest code and images and starts the app again: [Projects](../projects/projects.md).

The panel asks for a confirmation before a restart, a stop or a redeploy.

::docs-video{src="../../assets/single-server/statistics-containers-page.webm" title="Advanced > Containers of Immich: the services immich-server, immich-machine-learning, redis and database with their image, state running, uptime and ports, then the Actions menu with Start All, Restart, Stop All and Pull & Redeploy"}
::

### Public URLs

**Public URLs** is for container projects and developers. It forwards a domain of the project to a port of a service. The panel tells you if the domain is not one of this project or if the service already has a URL on that port.
