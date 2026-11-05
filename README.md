# [grealish.co.uk](https://grealish.co.uk/) Personal Website
[![Push to GitHub Pages on push to main](https://github.com/a-grealish/grealish.co.uk/actions/workflows/deploy.yml/badge.svg)](https://github.com/a-grealish/grealish.co.uk/actions/workflows/deploy.yml)

## Development
1. [Install Hugo](https://gohugo.io/installation/) (extended, v0.165.0 or newer)
2. Run a development server locally with `hugo server`

The site is self-contained: all templates live under `layouts/` and styles under `assets/css/`. There is no external theme or submodule to install.

## Deploy
Push to the `main` branch. `.github/workflows/deploy.yml` builds the static site with a pinned Hugo version and deploys it to [grealish.co.uk](https://grealish.co.uk) via GitHub Pages (repository `a-grealish/a-grealish.github.io`, using the `DEPLOY_TOKEN` secret).

## Google Analytics
To enable GA4, add the following to `config.yaml` (see the [Hugo services docs](https://gohugo.io/configuration/services/)):

```yaml
services:
  googleAnalytics:
    ID: G-XXXXXXXXXX
```

## Credits
Layouts and styles are derived from the [hugo-sustain](https://github.com/suyundukov/hugo-sustain) theme by Nurlan Suyundukov (MIT licence, see `licenses/hugo-sustain-LICENSE`).
