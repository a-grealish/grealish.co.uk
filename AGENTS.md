# AGENTS.md

## Dev Commands
- `hugo server` - Run local dev server
- `hugo` - Build static site

## Deploy
Push to `main` branch. CI (`.github/workflows/deploy.yml`) builds and deploys to grealish.co.uk via GitHub Pages.

## Structure
- `content/` - Markdown content (blog posts, pages)
- `static/` - Images and downloadable files
- `assets/css/main.css` - Site styles (built via the Hugo asset pipeline)
- `layouts/` - All templates (the former hugo-sustain theme is inlined here)

## Notes
- The site is self-contained: no theme submodule to init
- `config.yaml` - Main Hugo config
- Markdown in `content/` supports HTML (unsafe renderer enabled)
- Colours live in CSS custom properties at the top of `assets/css/main.css`; dark mode follows `prefers-color-scheme`
