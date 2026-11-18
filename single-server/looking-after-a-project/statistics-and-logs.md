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

Open the project and go to the **Monitoring** tab. **Visitors Chart** shows visits over the last week, month or year. Below it are the cards **Visits By Country**, **Continent**, **Region**, **Referrer**, **Device Operating System** and **Device Browser**.

The panel updates the statistics once a day. A new project shows `There is no data to display yet`. Check again the next day.

![The Monitoring tab of the Northwind Journal project: the Visitors Chart for Last Week, then Last Month, and the cards Visits By Country, Visits By Continent, Visits By Referrer, Visits By Device Operating System and Visits By Device Browser](../../assets/single-server/statistics-monitoring-tab.gif)

The **Project Details** tab also shows **Unique Visitors**, **Average Session Time** and **Bandwidth**. The session time is an estimate.

The **Visits By Country**, **Continent** and **Region** cards stay empty until the server owner downloads the local geolocation database with `pae geolocation:database update`. When they have data, they end with a link, `IP Geolocation by DB-IP`, to the source of that database.

## Usage of the server

Open the user menu and select **My Server**. The page shows three cards for the whole server: **RAM** and **Disk Space** with **Used** and **Available** in GB, and **CPU**. Below are graphs for **CPU Usage**, **Disk Throughput**, **Disk IOPS**, **Network Traffic** and **Network PPS**.

Choose **1 hour**, **12 hours** or **Live** in **Period**. **Live** is the last 5 minutes and is the default. It refreshes every 5 seconds and counts down with `Next update in <n> s`.

![My Server, Usage: RAM, CPU and Disk Space for the whole server, then the graphs Disk Throughput, Network Traffic and CPU Usage one after another](../../assets/single-server/statistics-server-usage.gif)

## Logs

Open the project and go to **Advanced > Logs**. Which tabs you see depends on the project:

- **Project Logs:** what was done to the project. A table with **User**, **Actions** and **Date**. You can search and sort by date. The page size is 2, 5, 10, 20 or 100.
- **Deploy Logs:** the deploy log of a container project. Pick a run from the list above the log. A running deploy has a **Live** badge.
- **WordPress Error Logs:** the error log of a WordPress site, as text. It stays empty unless `WP_DEBUG` and `WP_DEBUG_LOG` are on. It shows the last 100 entries and says `100 entries from the log are displayed. To see more, please download.` Use **Download** for the whole log.
- **PHP Logs:** the PHP errors, as a table with **Time**, **Level** and **Message**. A container project has no PHP log file here.
- **Webserver Logs:** the web server logs. Pick a file from the list at the top. An access log has **Time**, **IP Address**, **Request**, **Status** and **Size**. An error log has **Time**, **Level** and **Message**.

Every tab except **Project Logs** has a **Download** button next to the heading. It saves the log you are looking at. It is off while the log loads or is empty. A web server file the panel cannot show as text gets `You Cannot Preview the Contents of This File.` and the same button.

The PHP, web server and deploy logs show 15 rows a page, or 50 or 100.

![Advanced > Logs of the Northwind Journal WordPress project: Project Logs, then PHP Logs with No PHP Logs Were Found, then Webserver Logs with the access log rows and the Download button](../../assets/single-server/statistics-logs-tabs.gif)

A tab with nothing to show says `No PHP Logs Were Found...` or similar. The web server log often holds more detail.

### More detail

A file log loads from its newest end. When you go past the rows loaded, the table fetches the next older part. The search box filters only the rows already loaded.

## Containers

A container project has the page **Advanced > Containers**. The table lists each service with its image, its state, how long it has been running, the ports it publishes and its own actions. The columns are **Service**, **Image**, **State**, **Uptime**, **Ports** and **Actions**. The list refreshes every 15 seconds.

![Advanced > Containers of the ntfy project: the app service with its image, the state running, its uptime and port, the actions of the row, and the Actions menu at the top](../../assets/single-server/statistics-containers-page.gif)

- **Start All**, **Stop All**, **Restart** and **Pull & Redeploy** act on the whole project: [Projects](../projects/projects.md).
- Each service has its own **Start**, **Stop**, **Restart**, **Logs** and **Public URLs** actions. **Logs** shows the output of one service. An empty service shows `(no output)`.

The panel asks for a confirmation before a restart, a stop or a redeploy. A page with no services shows `No Containers Found` above `No containers available. They will appear here once a project is deployed.`

### Public URLs

**Public URLs** forwards a domain of the project to a port of that service. Two messages can stop you:

- The address is not a domain of this project: `The server name is not a domain of this project.`
- The service already has a URL on that port: `<name> already has a public URL on port <port>. Choose another port, or remove that URL first.`
