## Why

The Hugo site is freshly initialized with the Congo theme but has no content architecture, navigation, taxonomy, or deployment pipeline. Before real content can be authored, the site needs a structural skeleton (sections, config, cross-linking conventions, CI/CD) that matches the intended personal academic site at `duncanpoisson.github.io/site/`.

## What Changes

- Configure four top-level sections with Hugo navigation: Home, Blog, Research Journal, CV (plus About and Question Tracker).
- Establish a project-oriented Research Journal under `content/research/<project>/<entry>.md` with project landing pages and reverse-chronological entry listings.
- Enable per-post reading time and table of contents across Blog and Research entries.
- Configure a shared taxonomy (tags + categories) across Blog and Research Journal so cross-cutting themes surface in both sections.
- Populate `config/_default/params.toml`, `menus.toml`, and `hugo.toml` with Congo-appropriate settings, dark/light toggle, and author info.
- Seed placeholder content (clearly marked) for every section, including one example blog post, one example research project with one entry, and a Question Tracker skeleton.
- Add a GitHub Actions workflow (`.github/workflows/hugo.yml`) that builds on push to `main` and deploys to GitHub Pages at the `/site/` subpath.
- Preserve `layouts/partials/functions/warnings.html` (Hugo 0.160 compatibility shim) and avoid modifying files under `themes/congo/`.

## Capabilities

### New Capabilities
- `site-architecture`: Hugo site section layout, navigation, taxonomy, cross-linking conventions, and placeholder content.
- `deployment`: GitHub Actions build-and-deploy pipeline targeting GitHub Pages at the `/site/` subpath.

### Modified Capabilities
_(none — no existing specs)_

## Impact

- **Config**: `config/_default/hugo.toml`, `params.toml`, `menus.toml`, `markup.toml` (possibly `languages.toml`).
- **Content**: new `content/_index.md`, `content/blog/`, `content/research/`, `content/cv/`, `content/about/`, `content/questions/` trees.
- **CI/CD**: new `.github/workflows/hugo.yml` and GitHub Pages settings (Pages source must be set to "GitHub Actions" in repo settings after merge).
- **Theme**: no changes to `themes/congo/`. Existing override in `layouts/partials/functions/warnings.html` is preserved.
- **Quality gate**: `hugo --minify` must exit 0 with no `ERROR` lines.
