---
title: WHMCS Module
description: Troubleshoot PanelAlpha WHMCS integration issues including missing accounts, connection errors, and incorrect resource limits.
nav:
  group: Integration Issues
  order: 1
  title: WHMCS Integration
---


# WHMCS Module

This page covers common WHMCS integration issues in PanelAlpha.

### Account Not Visible in PanelAlpha

**Problem:** The account exists in WHMCS but does not appear in PanelAlpha.

**Solution:**
1. Verify that the product is set to **Automatically setup the product as soon as an order is placed** in WHMCS.
2. Manually run the **Create** action for the product in WHMCS to initiate the account setup in PanelAlpha.

### Connection Error

**Problem:** WHMCS cannot connect to PanelAlpha on port 8443. Common log entries include:

- `Connection error: Failed to connect to 158.220.XX.XX port 8443: Connection refused`
- `Connection Error: Failed to connect to xx.xx.xx.xxx port 8443 after 199 ms: Couldn't connect to server.`

**Solution:**
- Adjust the protocol (HTTP/HTTPS) and modify the **Secure** (SSL) connection setting in the configuration.
- Verify that the IP address configured in the WHMCS server settings is accurate and corresponds to PanelAlpha.

### Incorrect CPU and Memory Limits in WHMCS

**Question:** How can I adjust incorrect CPU and memory limits in WHMCS?

**Answer:** Refresh the module settings page in WHMCS and save the product to reload the current plan configuration from PanelAlpha.
