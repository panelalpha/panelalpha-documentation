---
title: Creating New Instance
description: Create a new WordPress instance in PanelAlpha with step-by-step onboarding and plan configuration.
nav:
  group: Client Area - Instances
  order: 2
---

# Creating New Instance

The process of adding a new instance can differ depending on the selected product and plan configuration.

In some setups, the flow is very short (for example, only location / domain input). In others, additional configuration steps are available.

Use the flow below as a reference. If one step does not appear in your panel, continue to the next one.

### Exemplary Workflow

1. Open **Add New** in the top-right corner of the Client Area.

![PanelAlpha Multi Server — Client Area Instances — New User Installation, Add New](../../assets/img/Client-Area-Instances/CNI_1.png)

2. Choose installation type:
   - **Create New WordPress Instance**
   - **Import Existing WordPress Instance**

   Then click **Continue**.

![PanelAlpha Multi Server — Client Area Instances — Create New WordPress Instance](../../assets/img/Client-Area-Instances/CNI_2.png)

3. When the installation flow opens, PanelAlpha fetches the available options for your service in the background. During this time a loading indicator is briefly visible. This step determines the target hosting server (applying the plan's [Server Assignment Rule](../system-configuration/plans.md#hosting-server), such as **Least Accounts**) and loads any user-configurable options specific to your plan.

4. Depending on plan / onboarding settings, one of the first steps can be:
   - choosing a datacenter / location (for example WP Cloud)
   - choosing a theme / template
   - or moving directly to domain / site configuration

5. If you have multiple hosting services, select the target hosting service for the new instance.

![PanelAlpha Multi Server — Client Area Instances — Choose Product](../../assets/img/Client-Area-Instances/CNI_4.png)

6. Configure installation details shown for your plan. Depending on onboarding mode, available fields can include:
   - Domain and related options
   - Clean installation vs clone / template
   - Language
   - Database name and prefix
   - WordPress admin credentials (username, password, email)

#### Choosing a Domain or Temporary Subdomain

If the plan has parent domains configured under **Domains → Subdomains** (see [Plans](../system-configuration/plans.md#domains)), the domain step can offer two options:

1. **Own domain** — enter or select a custom domain for the new site.
2. **Subdomain** — type a subdomain label (for example `mydomain`) and select a parent temporary domain from the plan (for example `wp1.example.com`). PanelAlpha builds the full hostname as `mydomain.wp1.example.com`.

Subdomain labels are validated against the plan subdomain blacklist and profanity rules.

![PanelAlpha Multi Server — Client Area Instances — Basic Configuration](../../assets/img/Client-Area-Instances/CNI_5.png)

![PanelAlpha Multi Server — Client Area Instances — Advanced Configuration](../../assets/img/Client-Area-Instances/CNI_5b.png)

   You cannot create a second instance at the same address on the same hosting account. If another instance already uses the same domain and directory, you see "Another instance already uses this address on this hosting account." Use a different subdirectory, for example `example.com/blog`.

7. Confirm by clicking **Continue** / **Create Site** (the label depends on the onboarding flow). Instance creation runs asynchronously and may take a few minutes.
8. After installation, you can still adjust domain settings using **Change Domain** in instance details.

![PanelAlpha Multi Server — Client Area Instances — Change Domain](../../assets/img/Client-Area-Instances/CNI_6.png)
