---
title: Configuration - Plugins & Themes
description: Manage plugin packages, theme categories, and custom assets in PanelAlpha Admin Area.
nav:
  group: Configuration
  order: 5
  title: Plugins & Themes
---

# Configuration - Plugins & Themes

This section centralizes plugin and theme assets used in onboarding and default-plan automation.
Use it to keep new sites consistent and reduce manual post-install cleanup.

![PanelAlpha Single Server — Admin Area — Plugins and themes packages list](../../assets/img/SSE/Admin-Area/Plugins_Themes_1.png)

## Main tabs

- **Packages**
- **Theme Categories**
- **Custom Plugins**
- **Custom Themes**

## Packages tab

![PanelAlpha Single Server — Admin Area — Creating plugin and theme package](../../assets/img/Themes-Plugins-And-Packages/Packages_1.png)

Use this tab to build reusable bundles of plugins and themes.

Typical workflow:
1. Build the package once.
2. Assign the package in **Default Plan**.
3. New instances inherit that stack automatically.

When to use:
- Standardized agency builds
- Vertical-specific starter stacks
- Controlled migration from ad-hoc plugin choices to managed bundles

## Theme Categories tab

![PanelAlpha Single Server — Admin Area — Theme categories management](../../assets/img/Themes-Plugins-And-Packages/Packages_2.png)

Use this tab to group themes into logical catalogs for onboarding.

This shortens decision time for non-technical users and keeps theme choices curated.

## Custom Plugins and Custom Themes tabs

![PanelAlpha Single Server — Admin Area — Custom plugins section](../../assets/img/Themes-Plugins-And-Packages/Packages_4.png)

Use these tabs to register internally approved assets (custom ZIPs/versions).

Use this when you need strict control over what can be installed across managed sites.

## Relation to Default Plan

![PanelAlpha Single Server — Admin Area — Default Plan plugins and themes tab](../../assets/img/General-Configuration/Plans_6.png)

Packages defined here can be assigned in:
- [Default Plan (Configuration)](default-plan.md)

This makes package policy enforceable during new-site provisioning.
