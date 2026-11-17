# Writing pages

This file is for anyone who edits the PanelAlpha documentation. What the repo is and how a
change reaches the site: [README](README.md).

## Making a change

1. Create a branch from `development`.
2. Edit or add pages. Check them on your computer: [Preview on your computer](#preview-on-your-computer).
3. Open a pull request to `development`. Once it is merged, the test site updates in about
   5 minutes.

## Page format

A page is plain GitHub Markdown, so it also reads well in the GitHub preview.

- **Notes.** Use GitHub alerts: `> [!NOTE]`, `> [!TIP]`, `> [!IMPORTANT]` and `> [!WARNING]`,
  then the text on lines that start with `> `. The site shows them as callouts.
- **Links to other pages.** Use relative links to the `.md` file, for example
  `[Stripe](../direct-billing/stripe.md#setup)` or `[Intro](../index.md)`. Do not write
  `/documentation/...` links. The build fails when the file does not exist. The text after `#` is
  the heading id used by the site (lower case, dashes, `_` before a leading digit). GitHub may
  not jump to it in the preview.
- **Images.** Put a new image in `assets/<product>/` and link it relatively. From
  `single-server/projects/git.md` that is `![Git card](../../assets/single-server/git-card.png)`.
- **Video and download button.** These two stay in the site format:
  `::docs-video{src="https://www.youtube.com/embed/..."}` followed by a line with `::`, and
  `:docs-button{href="https://..."}[Label]`. Their links must be full URLs.
- **Code.** Code blocks and inline code are published as they are.

## Frontmatter

Every page needs a `title`. The `nav` block puts the page in the sidebar. Leave it out for a page
that is not in the sidebar.

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

The root `index.md` holds the rest of the documentation home page: `title`, `lead`,
`description`, `seoDescription`, `community` (title, description, items with title, description,
href and label) and `cards` for cards that are not a section (external links), each with
`title`, `description`, `href` and `order`. Its text body is not published.

A new product is a new folder with its own `index.md`. It appears in the sidebar, the product
switcher, search and on the documentation home page.

## Preview on your computer

1. Clone panelalpha-homepage and run `git submodule update --init --remote` in it. This puts this
   repo in its `documentation/` folder.
2. Run `pnpm install` and `pnpm dev`. Open `http://localhost:3000/documentation`.
3. Edit the files in `documentation/`. That folder is a normal clone of this repo, so you can
   branch and commit there. The dev server rebuilds the pages when a file changes. Errors from
   the checks (missing title, broken link or image, unknown group) are printed in the terminal.

To run the checks only, run `pnpm exec nuxt prepare` in panelalpha-homepage.

## Repository settings

These are set once by a maintainer, under Settings > Secrets and variables > Actions.

| Name | Kind | Value |
|---|---|---|
| `HOMEPAGE_REPO` | variable | `panelalpha/panelalpha-homepage` |
| `HOMEPAGE_DEPLOY_TOKEN` | secret | fine-grained personal access token, see below |

Create the token under GitHub > Settings > Developer settings > Fine-grained tokens:

- Resource owner: `panelalpha`
- Repository access: `panelalpha-homepage` and `panelalpha-documentation`
- Permissions: Actions: Read and write (starts the site deploy), Contents: Read (lets the site
  build read this repo). GitHub adds Metadata: Read itself.

The same token is saved in panelalpha-homepage as `DOCS_READ_TOKEN`. **When the token expires,
the site stops updating.** Make a new one before the date set on it and save it in both places.
