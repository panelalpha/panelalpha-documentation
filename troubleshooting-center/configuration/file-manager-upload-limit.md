---
title: File Manager Upload Size Limit
description: Configure the File Manager upload size limit using the FILE_MANAGER_UPLOAD_MAX_FILESIZE environment variable applied to nginx and the API.
nav:
  group: Configuration Issues
  order: 5
  title: File Manager Upload Limit
---


# File Manager Upload Size Limit

The maximum allowed file size for uploads through the **File Manager** is controlled by the `FILE_MANAGER_UPLOAD_MAX_FILESIZE` environment variable. The same value is applied to the application API, the front-end, and the nginx reverse proxies in front of the application.

Previously the File Manager enforced a hard-coded **1 GB** limit in the API. Files larger than that were rejected both server-side and in the client interface, and the limit could not be changed without modifying the application code. The limit is now configurable, shared by all components, and the default remains **1 GB** unless overridden.

## Changing the Upload Size Limit

1. Open the environment file:
   ```bash
   sudo nano /opt/panelalpha/app/.env
   ```
2. Add or update the `FILE_MANAGER_UPLOAD_MAX_FILESIZE` line. Accepted formats follow standard PHP/byte-size notation and are parsed both by nginx and by the API:
   ```bash
   FILE_MANAGER_UPLOAD_MAX_FILESIZE=1G
   ```
   Other valid values include `1024M`, `2048M`, `3G`, and `512M`.
3. Save the file and recreate the `api` and `nginx` containers so the entrypoint re-renders the nginx templates and reloads the configuration. A plain `docker compose restart` is **not** sufficient because template substitution only happens at container start:
   ```bash
   cd /opt/panelalpha/app && docker compose up -d
   ```
   The nginx configuration files are shipped as templates (`*.tpl`), and the `client_max_body_size` directive is rendered into the main nginx proxy and the API service nginx at container start. Recreating the containers via `up -d` re-renders the templates and reloads both `api` and `nginx` — there is no need to edit any rendered config file by hand.
4. Verify the change by checking the rendered nginx configuration:
   ```bash
   cd /opt/panelalpha/app && docker compose exec nginx grep -r client_max_body_size /etc/nginx/conf.d/
   ```
   The displayed value should match the one set in `.env`.

## Troubleshooting

### Uploads Larger Than the Configured Size Fail with HTTP 413

**Problem:** A file accepted by the File Manager UI is rejected during upload with `413 Request Entity Too Large`.

**Solution:** Verify that `FILE_MANAGER_UPLOAD_MAX_FILESIZE` in `/opt/panelalpha/app/.env` is set high enough for the file you are uploading (the default is `1G`, with a fallback of `1024M` when unset), then recreate the `api` and `nginx` containers:

```bash
cd /opt/panelalpha/app && docker compose up -d
```

The upload limit is enforced by nginx via `client_max_body_size`; PHP itself is configured with `upload_max_filesize = 0` and `post_max_size = 0` (no limit), so the nginx value is the effective ceiling.

### The File Manager UI Rejects Files Smaller Than the Server Accepts

**Problem:** The front-end refuses a file while a direct API request for the same file would succeed.

**Solution:** The front-end reads the limit from the API configuration. If the API was restarted after the change but the nginx templates were not re-rendered (or vice versa), the two values may temporarily disagree. Restart the full stack listed in the previous step and reload the File Manager page in the browser to pick up the new limit.

### The Configured Value Is Not Applied After Editing `.env`

**Problem:** Changing `FILE_MANAGER_UPLOAD_MAX_FILESIZE` has no effect on uploads.

**Solution:**

1. Confirm the file was saved in the expected location:
   ```bash
   sudo cat /opt/panelalpha/app/.env | grep FILE_MANAGER_UPLOAD_MAX_FILESIZE
   ```
2. Confirm the application containers were recreated after the change. The substitution into nginx templates and the API only happens at container start, so use `docker compose up -d` (not just `restart`).
3. Confirm the rendered nginx configuration reflects the new value:
   ```bash
   cd /opt/panelalpha/app && docker compose exec nginx grep -r client_max_body_size /etc/nginx/conf.d/
   ```

If the rendered nginx config still shows the old value, the containers did not pick up the new `.env` — re-run `docker compose up -d` from the change procedure above to recreate them.
