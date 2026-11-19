---
title: Database Access Configuration
description: Configure database access for PanelAlpha including phpMyAdmin setup, connection troubleshooting, and CLI access.
nav:
  group: Configuration Issues
  order: 4
  title: Database Access
---


# Database Access Configuration

This guide covers advanced database access configuration for PanelAlpha, including setting up phpMyAdmin and troubleshooting database connectivity issues.

## phpMyAdmin Installation

phpMyAdmin provides a web-based interface for managing your PanelAlpha database.

### Installation Process

1. **Access your server** via SSH with root privileges:
   ```bash
   ssh root@your-server-ip
   ```
2. **Install phpMyAdmin** using Docker:
   ```bash
   docker run --name appphpmyadmin -d --link app-database-api-1 -p 7070:80 --network app_default -e PMA_HOST=database-api.palocal phpmyadmin
   ```

### Command Parameters Explained

- **`--name appphpmyadmin`**: Sets the container name for easy management
- **`-d`**: Runs the container in detached mode (background)
- **`--link app-database-api-1`**: Links to the PanelAlpha database container
- **`-p 7070:80`**: Maps phpMyAdmin to port 7070 on your server
- **`--network app_default`**: Uses the PanelAlpha Docker network
- **`-e PMA_HOST=database-api.palocal`**: Sets the database host

### Optional Parameters

You can add the `--rm` parameter for temporary access:

```bash
docker run --rm --name appphpmyadmin -d --link app-database-api-1 -p 7070:80 --network app_default -e PMA_HOST=database-api.palocal phpmyadmin
```

**`--rm`**: Automatically removes the container when stopped. Use this for temporary database access.

## Accessing phpMyAdmin

### Web Access

1. Open your web browser.
2. Navigate to: `http://YOUR_SERVER_IP:7070`
3. Use your database credentials to log in.

**Example:** If your server IP is `192.168.1.100`, access: `http://192.168.1.100:7070`

### Custom Port Configuration

If port 7070 is already in use, change it to an available port:

```bash
# Using port 8080 instead
docker run --name appphpmyadmin -d --link app-database-api-1 -p 8080:80 --network app_default -e PMA_HOST=database-api.palocal phpmyadmin
```

## Database Connection Troubleshooting

### Common Connection Issues

#### Container Not Found

**Error:** Cannot link to container

**Solution:**
1. Verify the database container is running:
   ```bash
   docker ps | grep database
   ```
2. Check the correct container name:
   ```bash
   docker ps --format "table {{.Names}}\t{{.Status}}"
   ```

#### Port Already in Use

**Error:** Port 7070 is already allocated

**Solution:**
1. Check which process is using the port:
   ```bash
   netstat -tulpn | grep 7070
   ```
2. Use a different port or stop the conflicting service.

#### Network Issues

**Error:** Cannot connect to network

**Solution:**
1. Verify the Docker network exists:
   ```bash
   docker network ls | grep app_default
   ```
2. Restart the PanelAlpha containers if needed.

### Database Credentials

Default database connection details for the PanelAlpha application database:

- **Host:Port**: `database-api.palocal:3306` (internal Docker network hostname)
- **Username**: `panelalpha` (the `DB_USERNAME` key in `/opt/panelalpha/app/.env-api`)
- **Password**: the value of `API_MYSQL_PASSWORD` from `/opt/panelalpha/app/.env`. This password is injected into the containers by docker-compose; in `.env-api` the `DB_PASSWORD=` field is intentionally left empty.
- **Database**: `panelalpha`

To connect from outside the containers, use the `database-api` service directly:

```bash
docker compose -f /opt/panelalpha/app/docker-compose.yml exec database-api mariadb -u panelalpha -p
```

> [!NOTE]
> **Note:** The phpMyAdmin instance shipped with PanelAlpha is used for SSO access to **customer databases** hosted on hosting servers — it is not intended for managing the PanelAlpha application's own database. Use the CLI command above for application database access.

## Container Management

### Managing the phpMyAdmin Container

**Start the container:**
```bash
docker start appphpmyadmin
```

**Stop the container:**
```bash
docker stop appphpmyadmin
```

**Remove the container:**
```bash
docker rm appphpmyadmin
```

**View container logs:**
```bash
docker logs appphpmyadmin
```

## Security Considerations

### Access Control

1. **Restrict access** by IP address if possible.
2. **Use firewall rules** to limit port 7070 access.
3. **Remove phpMyAdmin** when not needed for security.
4. **Use strong database passwords**.

### Temporary Access

For security, consider using the `--rm` flag for temporary database access:

```bash
# Temporary access - container removes itself when stopped
docker run --rm --name temp-phpmyadmin -d --link app-database-api-1 -p 7070:80 --network app_default -e PMA_HOST=database-api.palocal phpmyadmin
```

## Alternative Database Access

### Command Line Access

Access the database directly via command line:

```bash
# Access MySQL/MariaDB directly
docker compose -f /opt/panelalpha/app/docker-compose.yml exec database-api mysql -u root -p
```

### Database Backup

Create database backups for troubleshooting:

```bash
# Create a database backup
docker compose -f /opt/panelalpha/app/docker-compose.yml exec database-api mysqldump -u root -p panelalpha > backup.sql
```

## Common Issues

### phpMyAdmin Not Loading

**Problem:** The phpMyAdmin interface does not load in the browser when accessing it through PanelAlpha.

**Solution:**
1. Check if the container is running: `docker ps`
2. Verify port accessibility: `telnet your-server-ip 7070`
3. Check container logs: `docker logs appphpmyadmin`

### Database Connection Failed

**Problem:** PanelAlpha or phpMyAdmin cannot connect to the database, causing errors during operations.

**Solution:**
1. Verify the database container is running.
2. Check network connectivity between containers.
3. Verify database credentials.

### Performance Issues

**Problem:** phpMyAdmin or database operations are slow or unresponsive, especially with large datasets.

**Solution:**
1. Allocate more resources to the phpMyAdmin container.
2. Check server resource usage.
3. Consider using command-line tools for large operations.

## Getting Help

If you encounter database access issues:

1. Check the [Common Error Solutions](../common/common-error-solutions.md) guide.
2. Review Docker and database logs for error messages.
3. Contact our [support team](https://manage.panelalpha.com/contact) for assistance.
