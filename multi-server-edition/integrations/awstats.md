---
title: AWStats Integration
description: Use the built-in AWStats integration for automatic visitor and bandwidth statistics in PanelAlpha.
nav:
  group: Integrations
  order: 2
  title: AWStats
---

# AWStats Integration

## Overview

AWStats is a web analytics tool that analyzes server logs to provide detailed statistics about your websites. Starting from PanelAlpha version 1.4.1, AWStats integration is automatically enabled and immediately begins collecting data without requiring additional configuration.

## How It Works

The AWStats integration functions through the following processes:

1. **Automatic Log Collection**: The system retrieves webserver logs and analyzes them using AWStats.
2. **Daily Processing**: Logs are processed and stored in the `storage/app/awstats` directory.
3. **Seamless Data Integration**: Processed data is automatically displayed in relevant sections of PanelAlpha.

## Data Availability

Collected statistics are displayed in multiple locations:

### Client Area
- **Instance > General**: Summary statistics including visitor counts and bandwidth usage
- **Instance > Monitoring**: Detailed historical data about visitors and bandwidth consumption

### Admin Area
- **Instance > Bandwidth**: Bandwidth usage statistics
- **Instance > Unique Visitors**: Visitor tracking metrics

## Background Processes

AWStats integration relies on two primary background jobs:

1. **CacheInstanceDetails**: This daily job caches visitors and bandwidth data. For bandwidth data, the system retrieves information from AWStats if the hosting server does not provide it directly. This is important for DirectAdmin, Engine, and WP Cloud platforms.

2. **RefreshReportData**: This job runs once daily to retrieve logs and analyze them, ensuring statistics are up-to-date.

## Benefits

- **Zero Configuration**: Works automatically without setup
- **Comprehensive Statistics**: Provides detailed visitor and bandwidth data
- **Improved Monitoring**: Enhances monitoring in both Client and Admin areas
- **Server Compatibility**: Works across different hosting server types, including those that previously provided limited statistical data

Since this integration is enabled by default and requires no configuration, you can immediately benefit from enhanced statistical reporting.
