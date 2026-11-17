# PanelAlpha documentation

Source of the documentation published at panelalpha.com/documentation. This repo holds only
the pages (Markdown) and their images. The site itself is built by
[panelalpha-homepage](https://github.com/panelalpha/panelalpha-homepage).

The pages still live in panelalpha-homepage and in the old VuePress repo. They move here in a
later step.

## Layout

One folder per product. A new product is a new folder.

```
index.md               documentation home page
single-server/
multi-server/
  troubleshooting/
panelalpha-engine/
  v1/                  Engine 1.x
```

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
