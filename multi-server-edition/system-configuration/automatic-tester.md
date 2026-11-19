---
title: Automatic Tester
description: Run the PanelAlpha Automatic Tester to validate connections, plans, and functionality after configuration or updates.
nav:
  group: System Configuration
  order: 14
---

# Automatic Tester

## Introduction

Every company uses PanelAlpha differently, connecting it with multiple back-end solutions. To ensure all configurations work correctly, PanelAlpha provides the Automatic Tester.

After configuring PanelAlpha, use the Automated Tester to verify that all connections, plans, and functionalities work as expected. It is also recommended to use this tester after every PanelAlpha update.

**Currently, the Automatic Tester is launched manually.** Please wait for our team to initiate tests, which usually takes a few hours during business days.

## What is Automatic Tester?

The Automatic Tester is an automated testing tool that validates the functionality and performance of your PanelAlpha installation. It simulates actual user interactions, executing various scenarios and operations to verify that all components function correctly.

## Covered Functionalities

The Automatic Tester covers:

- **Client Area Actions**:
  - **Instance Management**:
    - Fetching and caching hosting plans
    - Creating test users for each plan
    - Assigning plans to users
    - Fetching and caching available integrations
    - Selecting a hosting plan, configuring an instance, and verifying creation
    - Filling out forms, selecting domains, and verifying installation
    - Importing an existing WordPress site
    - Configuring FTP/SFTP and database access
    - Verifying import correctness
  - **My Instances Actions**:
    - Testing all actions available in My Instances (based on plan privileges)
    - Testing site preview, sharing instances, and accessing WP Admin
    - Verifying staging creation, copying instances, and clearing cache
    - Changing instance name and verifying information
    - Creating, restoring, and deleting backups
    - Configuring automatic backup schedules
    - Searching, installing, activating, deactivating, and deleting plugins
    - Managing Cloudflare, Edge Cache, and instance security
    - Configuring automatic updates and modes
    - Managing WordPress users, themes, configuration, and logs
  - **My Hosting Actions**:
    - Testing all actions available in My Hosting (based on plan privileges)
    - Viewing service statistics
    - Adding, configuring, and managing domains
    - Creating, editing, and deleting SFTP/SSH and FTP accounts
    - Creating databases and managing MySQL users
    - Verifying PHPMyAdmin access via SSO
    - Adding, editing, and deleting cron jobs
    - Checking the File Manager
    - Configuring DNS records (A, AAAA, CNAME, MX, TXT)
    - Creating, configuring, and deleting email accounts
    - Configuring automatic email forwarding

**Important notes:**
- The tester does not create new plans or connections; it operates on existing configuration.
- The tester does not perform tests on existing clients, accounts, or instances. It creates its own test data.

Future updates will include Admin Area functionality testing.

## How to Run Automatic Tester

1. Ensure your PanelAlpha system is fully configured.
2. Navigate to: [Automatic Tester](https://manage.panelalpha.com/automatic-tester.php)
3. Provide the required information.
4. Submit the form to initiate testing.

## Test Results

After completion, a comprehensive report is generated and sent via a support ticket.

The report contains:
- Tests that were run
- Results of each test
- Any issues or errors detected

If errors are detected, the PanelAlpha team will analyze them and work to resolve the problems.

### See an Example Test Result

Download an example test result report: [Download Example Test Result](https://github.com/panelalpha/automatic-tester/raw/main/docs/example-test-result.pdf)

## Benefits of Using Automatic Tester

- Verify that your PanelAlpha installation is functioning correctly
- Automatically test multiple functionalities without manual intervention
- Identify potential issues before they affect users
- Ensure all configurations work as expected
