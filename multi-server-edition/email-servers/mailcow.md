---
title: Mailcow
description: Configure Mailcow as an email server in PanelAlpha and assign it to plans for client area management.
nav:
  group: Email Servers
  order: 1
---

# Mailcow

PanelAlpha users can manage email addresses and email forwarders directly from the client area. To enable these features, set up an email server in the plan.

If cPanel, DirectAdmin, or Plesk is set up as a hosting server, you do not need a separate email server, as these can function as the hosting, email, and DNS server simultaneously.

If **PanelAlpha Engine** is the hosting server, configuring Mailcow is mandatory, as it does not provide email services.

## Configuration

To configure Mailcow, go to **Admin Panel → Configuration → Servers → Email Servers**.

Press **Add**, and a short form will appear.

Enter the email server name and select **Mailcow** from the type dropdown. Then provide the API URL and API Key.

### API Details

Where to find the necessary API details:

1. Open Mailcow and log in as administrator.
2. Scroll down and open the **API** section.
3. Enter the IP address from which you want to access the API, or disable the API check.
4. Select **Activate API** and save.
5. Copy the API key.

Go back to the PanelAlpha email server configuration, paste the API Key and URL, and save. Mailcow is now ready.

## Plan Configuration

Assign the email server to a plan so that clients can manage mailboxes and forwarders from the Client Area.

1. Go to **Admin Area → Configuration → Plans**, then open an existing plan or create a new one.
2. Open the **Email** tab.
3. In **Server Type**, select **Mailcow**.
4. In **Server**, select the Mailcow instance you configured above.
5. (Optional) Configure **Email Account Config** to define mailbox defaults for the plan.
6. (Optional) Expand **Advanced Configuration** to enable:
   - **Auto set up SPF & DKIM records** — automatically add SPF and DKIM DNS records for email domains.
   - **Display SPF & DKIM configuration** — show SPF/DKIM details to clients.
   - **Auto delete email domains** — remove email domains from the email server when the service is deleted (external servers only).
7. Save the plan.

Email domain synchronization is a separate action performed from **Configuration → Servers → Email Servers** (Synchronize Domains), not from the plan.
