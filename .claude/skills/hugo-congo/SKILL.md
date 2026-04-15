---
name: hugo-congo
description: "Authoring and configuration conventions for this Hugo + Congo theme site. Use when adding content, tweaking config, or diagnosing build errors."
---

# Hugo + Congo

This site is built with [Hugo](https://gohugo.io/) using the [Congo theme](https://jpanther.github.io/congo/), installed as a git submodule at `themes/congo/`. Final production URL: `https://duncanpoisson.github.io/site/`.

## Hard rules

- **Do NOT modify files under `themes/congo/`.** It's a submodule; changes will be lost on next update.
- **Do NOT delete `layouts/partials/functions/warnings.html`.** This is a local override that patches a Hugo 0.160 incompatibility in Congo.
- **Do NOT invent `baseURL` overrides.** The production baseURL is set in `config/_default/hugo.toml`. All internal links must resolve under `/site/`.
- **Do NOT write final content in this repo without explicit request.** Content files are placeholders unless the user asks otherwise.

## Directory layout

```
config/_default/
  hugo.toml         # core config, baseURL, taxonomies, outputs
  params.toml       # Congo-specific params (appearance, article settings, author)
  menus.toml        # top-level navigation
  markup.toml       # goldmark / TOC / highlighting config
content/
  _index.md                           # homepage
  blog/_index.md, blog/<post>.md      # dated posts
  research/_index.md                  # lists projects
  research/<project>/_index.md        # project landing
  research/<project>/<entry>.md       # individual research entries
  cv/_index.md
  about/_index.md
  questions/_index.md
layouts/partials/functions/warnings.html   # Hugo 0.160 compat shim — keep
themes/congo/                               # submodule — do not touch
```

## Cross-linking between sections

Use Hugo's `ref` / `relref` shortcodes — never raw paths. Broken targets fail the build, which is the point.

```markdown
See the companion post: [Example Post]({{< ref "/blog/example-post" >}}).
```

`ref` returns an absolute URL (correct for the `/site/` subpath). `relref` returns a path relative to the current page. Default to `ref` unless you specifically need relative.

## Taxonomy

Site-wide `tags` and `categories` are shared across Blog and Research Journal. Same tag on a blog post and a research entry produces one aggregated `/tags/<term>/` page. This is intentional — if it becomes noisy, revisit before adding per-section taxonomy.

## Reading time + TOC

Global defaults are set in `params.toml` under `[article]` (`showReadingTime = true`, `showTableOfContents = true`). Individual posts override via frontmatter if needed.

## Build & validation

```bash
hugo --minify           # production build; must exit 0, no ERROR lines
hugo server -D          # dev server, includes drafts
```

Quality gate passes if `hugo --minify` completes with exit code 0 **and** stdout/stderr contain no lines beginning with `ERROR`.

## Deployment

GitHub Actions workflow at `.github/workflows/hugo.yml` deploys to GitHub Pages on push to `main`. After first merge, set **Settings → Pages → Source = "GitHub Actions"** in the GitHub UI (one-time). CI must check out submodules recursively for the theme to be available.

## Common pitfalls

- **Submodule missing in CI** → theme not found, build fails. Fix: `submodules: recursive` in `actions/checkout`.
- **Raw internal links** (`[x](/blog/foo)`) → work locally but break under `/site/` subpath. Use `ref`/`relref`.
- **Removing the `warnings.html` shim** → Hugo 0.160 build errors in Congo. Keep it.
- **Editing under `themes/congo/`** → changes lost on submodule update. Override via `layouts/` at repo root instead.
- **`draft: true` in frontmatter** → not rendered by `hugo --minify`. Fine for work-in-progress, but remember to flip it.
