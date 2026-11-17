---
title: Automatic SSL Issues
description: Resolve PanelAlpha automatic SSL issues including orders stuck on DNS propagation and orders marked as skipped.
nav:
  group: Configuration Issues
  order: 3
  title: Automatic SSL
---


# Automatic SSL Issues

### Order Stuck on "Awaiting DNS Propagation"

**Problem:** An SSL order does not complete and stays in pending or propagation status.

**Solution:**
1. Verify required DNS records exist (root and `www`, plus staging host if applicable).
2. If using Cloudflare or another proxy, gray-cloud the records during validation so Let's Encrypt can reach the origin.
3. Check that the DNS provider API token has permissions to edit DNS and zone settings.
4. Click **Process Now** after fixing DNS to retry the order.

### Order Marked as "Skipped"

**Problem:** An SSL order is skipped because another certificate is already served (for example, a Cloudflare edge certificate).

**Solution:**
1. If the Cloudflare certificate is acceptable, no action is required.
2. If you need a PanelAlpha-issued certificate, temporarily disable the proxy and reprocess the order.
