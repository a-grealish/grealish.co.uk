# AGENTS.md

## Dev Commands
- `hugo server` - Run local dev server
- `hugo` - Build static site

## Deploy
Push to `main` branch. CI (`.github/workflows/deploy.yml`) builds and deploys to grealish.co.uk via GitHub Pages.

## Structure
- `content/` - Markdown content (blog posts, pages)
- `static/` - Assets (images, CSS, files)
- `layouts/` - Custom templates
- `themes/hugo-sustain/` - Theme (git submodule)

## Notes
- Theme is a git submodule - run `git submodule update --init` after clone
- `config.yaml` - Main Hugo config
- Markdown in `content/` supports HTML (unsafe renderer enabled)
