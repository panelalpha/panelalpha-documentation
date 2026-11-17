# PanelAlpha documentation

Source of the documentation published at panelalpha.com/documentation. This repo holds only
the pages (Markdown) and their images. The site itself is built by
[panelalpha-homepage](https://github.com/panelalpha/panelalpha-homepage).

## Layout

One folder per product. A new product is a new folder.

```
index.md               texts and external cards of the documentation home page (not a page)
assets/                images and gifs, same sub-folders as the image links
single-server/         one folder per product ("section")
multi-server/
panelalpha-engine/
  v1/                  Engine 1.x
troubleshooting-center/
```

Each section folder has an `index.md` with the section settings (see below). Folder pages are
named `index.md`. The page URL is the file path without `.md`, under `/documentation`:
`multi-server/billing/stripe.md` is `/documentation/multi-server/billing/stripe`.

## Branches

| Branch | Site |
|---|---|
| `development` | test site |
| `main` | panelalpha.com (not connected yet) |

Work in your own branch and open a pull request to `development`.

## How a change reaches the site

1. A push to `development` runs `.github/workflows/update-site.yml`.
2. That workflow starts `deploy.yml` in panelalpha-homepage on its `development` branch.
3. panelalpha-homepage runs its checks, builds the site and deploys it to the test site. This
   takes about 5 minutes. The result is in the Actions tab of panelalpha-homepage.

## Settings in GitHub

Settings > Secrets and variables > Actions:

| Name | Kind | Value |
|---|---|---|
| `HOMEPAGE_REPO` | variable | `panelalpha/panelalpha-homepage` |
| `HOMEPAGE_DEPLOY_TOKEN` | secret | fine-grained personal access token, see below |

The token: GitHub > Settings > Developer settings > Fine-grained tokens > Generate new token.

- Resource owner: `panelalpha`
- Repository access: only `panelalpha-homepage`
- Permissions: Actions: Read and write (Metadata: Read is added by GitHub)

The token expires on the date chosen when it was made. After that the site stops updating
until someone makes a new one and saves it in `HOMEPAGE_DEPLOY_TOKEN`.

## Writing pages

A page is plain GitHub Markdown, so it also reads well in the GitHub preview.

- Notes: use GitHub alerts. `> [!NOTE]`, `> [!TIP]`, `> [!IMPORTANT]` and `> [!WARNING]`, then the
  text on lines that start with `> `. The site shows them as callouts.
- Links to other pages: relative links to the `.md` file, for example
  `[Stripe](../direct-billing/stripe.md#setup)` or `[Intro](../index.md)`. Do not write
  `/documentation/...` links. The build fails when the file does not exist. The text after `#` is
  the heading id used by the site (lower case, dashes, `_` before a leading digit). GitHub may
  not jump to it in the preview.
- Images: put the file in `assets/` and link it relatively, for example
  `![Settings](../../assets/img/Getting-Started/settings.png)`.
- Video and download button stay as in the site format:
  `::docs-video{src="https://www.youtube.com/embed/..."}` followed by a line with `::`, and
  `:docs-button{href="https://..."}[Label]`. Their links must be full URLs.
- Code blocks and inline code are not changed.

### Frontmatter

Every page needs a `title`. The `nav` block puts the page in the sidebar. Leave it out for pages
that are not in the sidebar.

```yaml
---
title: "Stripe"                # page title (H1, browser title)
description: "Connect Stripe"  # short text for search engines
nav:
  group: Direct Billing        # group title, declared in the section index
  order: 3                     # position in that group, starting at 1, unique per group
  title: Stripe Setup          # only when the sidebar label differs from the title
  section: multi-server        # only when the group is in another section than the page folder
---
```

`nav` can be a list when a page shows up in more than one group. Two pages in one group cannot
have the same `order`, and a `group` that the section does not declare fails the build.

The first file of a section, `<section>/index.md`, also holds the section settings:

```yaml
section:
  title: Single Server         # name in the sidebar and the product switcher
  order: 1                     # order of the sections
  groups:                      # order and options of the groups; pages join a group by its title
    - title: Getting Started
      path: getting-started/index.md  # optional: page the group heading links to,
                                      # relative to the section folder
      collapsable: false              # only when the group must stay open
      dividerBefore: true             # only when a divider line goes above the group
card:                          # card of this section on the documentation home page
  title: Single Server Edition
  subtitle: Open source        # optional
  description: Deploy and manage PanelAlpha on one server.
  order: 1                     # position among the cards
```

The root `index.md` holds the rest of the home page: `title`, `lead`, `description`,
`seoDescription`, `community` (title, description, items with title, description, href, label)
and `cards` for cards that are not a section (external links), each with `title`, `description`,
`href` and `order`. Its text body is not published.

## Preview on your computer

1. Clone panelalpha-homepage and run `git submodule update --init --remote` in it. This puts this
   repo in its `documentation/` folder.
2. Run `pnpm install` and `pnpm dev`. Open `http://localhost:3000/documentation`.
3. Edit the files in `documentation/` (that folder is a normal clone of this repo, so you can
   branch and commit there). The dev server rebuilds the pages when a file changes. Errors from
   the checks (missing title, broken link or image, unknown group) are printed in the terminal.

To run the checks only, run `pnpm exec nuxt prepare` in panelalpha-homepage.
