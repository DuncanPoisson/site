---
id: sit-k1d8
status: closed
deps: []
links: []
created: 2026-04-15T23:48:32Z
type: task
priority: 0
assignee: Duncan Haddock
parent: sit-g16i
---
# Configure Hugo site: params, taxonomy, menus, markup

Fill out config/_default/params.toml (Congo appearance, author, showReadingTime, showTableOfContents, dark/light toggle); add [taxonomies] and [outputs] to hugo.toml; create menus.toml with Home/Blog/Research Journal/CV/About; review markup.toml for TOC; add languages.toml only if Congo requires.

## Acceptance Criteria

hugo --minify exits 0 with no ERROR lines; hugo server -D starts clean; menu entries render; /tags/ and /categories/ pages exist; dark/light toggle visible.

## Skills

- hugo-congo
- backpressure
- testing-principles

## Design

- Files: `config/_default/params.toml`, `config/_default/hugo.toml`, `config/_default/menus.toml`, `config/_default/markup.toml` (possibly `languages.toml`).
- Set `[article] showReadingTime = true`, `showTableOfContents = true` globally (design decision #4).
- Enable `[taxonomies] tag = "tags"` and `category = "categories"` at site level (decision #2 — shared across Blog and Research).
- Add `[outputs] home = ["HTML", "RSS"]`.
- Menu entries (name, url, weight): Home `/`, Blog `/blog/`, Research `/research/`, CV `/cv/`, About `/about/`.
- Preserve `layouts/partials/functions/warnings.html`; do not modify `themes/congo/`.
- Validation: `hugo --minify` then `hugo server -D` — visit routes manually.


## Notes

**2026-04-16T19:23:23Z**

Configured hugo.toml (taxonomies, outputs, pagination, robots), params.toml (Congo appearance, article, header/footer, homepage layout, dark/light toggle), menus.toml (Home/Blog/Research/CV/About), markup.toml (TOC, highlight). Build passes clean, /tags/ and /categories/ pages render.
