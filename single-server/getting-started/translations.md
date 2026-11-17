---
title: Translations
description: Install translation files, sync notifications, and change the default language in PanelAlpha.
nav:
  group: Getting Started
  order: 4
---

# Translations

This guide explains how to enable and manage interface translations.
By default, PanelAlpha uses English, but the Client Area language can be changed.

## Install Translation Files

Run the automatic sync script:

```bash
curl -fsSL -o /opt/panelalpha/sync-translations.sh https://raw.githubusercontent.com/panelalpha/PanelAlpha-Translations/refs/heads/main/scripts/sync-translations.sh
bash /opt/panelalpha/sync-translations.sh --lang "de"
```

Replace `de` with your target language code (`de`, `es`, `fr`, `it`, `pt_BR`).

![PanelAlpha Single Server — Getting Started — Translation files structure](../../assets/img/Getting-Started/translations_1.png)

## Sync Notifications

```bash
docker compose -f /opt/panelalpha/app/docker-compose.yml exec -T api php artisan notifications:sync
```

## Change Default Language

1. Open Admin Area (`:8443`).
2. Navigate to `Configuration -> General -> Client Area`.
3. Set **Default Language**.
4. Save.

![PanelAlpha Single Server — Getting Started — Setting default client area language in admin area](../../assets/img/Getting-Started/translations_2.png)

Users can still select their own language in Client Area.

![PanelAlpha Single Server — Getting Started — Language switcher on the client area login page](../../assets/img/Getting-Started/translations_3.png)

## Troubleshooting

- Language does not appear in the dropdown: verify translation folder name and permissions.
- Emails are still in old language: run `notifications:sync` again.
