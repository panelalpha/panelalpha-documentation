---
title: Free Trial Period
description: Offer free WordPress hosting trials using PanelAlpha Direct Billing with Stripe or WHMCS.
nav:
  group: Billing Scenarios
  order: 2
---

# Free Trial Period

Use **Direct Billing** to offer a trial WordPress site, then convert the customer on your own checkout page.

## Stripe

Stripe is the path when you want invoices and Pay Now in the Client Area.

1. Enable [Direct Billing](../direct-billing/index.md) and connect Stripe.
2. On a plan, open **Billing**, set **Product Type** to **Trial**, and copy the trial registration links.
3. Set **Upgrade URL** to your page that starts Stripe Checkout. PanelAlpha appends `trial_service_id`, `trial_email`, and `trial_instance_count`.
4. Map paid plans to Stripe prices so the webhook can upgrade the service.

Full use cases: [Stripe Integration](../direct-billing/stripe.md#how-to-offer-trials).

## WHMCS

1. Enable Direct Billing and select WHMCS.
2. Configure the trial plan Billing tab (length, suspend/remove, upgrade URL).
3. Use [WHMCS Direct Billing](../billing-systems-integrations/whmcs.md#direct-billing) for the API token and usage billing.

SSO from WHMCS into PanelAlpha still applies when you use the module.

## Typical workflow

1. The user opens a trial registration link.
2. They create the first WordPress site (Quick Onboarding is a good default).
3. When the trial ends, they follow the upgrade URL and pay in Stripe or WHMCS.
4. PanelAlpha (Stripe webhook) or WHMCS (module) moves them to the paid plan.
