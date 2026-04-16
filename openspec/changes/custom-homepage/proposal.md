## Why

The homepage currently uses Congo's default `page` layout with placeholder copy and the site is still titled "Duncan Poisson". The site needs a real landing experience that introduces Duncan, frames the site's three working areas (Blog, Journal, Projects), and reflects the owner's actual name and voice. The design-overhaul change established the palette, navigation, and section layouts; this change builds the landing page on top of that foundation.

## What Changes

- **BREAKING** Site title and author name rename: `"Duncan Poisson"` → `"Duncan Xavier Haddock"` across `config/_default/hugo.toml`, `config/_default/params.toml`, `content/_index.md`, and `IMPLEMENTATION_PLAN.md`. The header/nav continues to display the site title (now in primary green `#429347`).
- New custom homepage layout with four burnt-orange-divided sections:
  1. **Hero** — Duncan's photo (left on desktop, top on mobile) alongside a name heading in primary green and a short self-introduction.
  2. **What is this website?** — green heading, bulleted research questions, and three clickable green rectangular cards (rounded corners) linking to `/blog/`, `/journal/`, and `/projects/` with subtitles describing each.
  3. **Who am I?** — green heading, background paragraphs, and one "More of me" green card linking to a placeholder Linktree URL.
  4. **Quote** — centered italic Angela Davis blockquote.
- Final homepage copy (no more placeholders) written exactly as specified in the project spec.
- Reference a photo at `static/images/duncan.jpg` (placeholder file created; the user will swap the real photo in later).
- Layout is mobile-responsive: hero stacks vertically, bubble row collapses to a column.
- Burnt orange `#C74D20` horizontal dividers (2–3px solid) separate sections.

## Capabilities

### New Capabilities

- `site-identity`: Canonical site title, author name, and header display color. Establishes where the name appears and how it is rendered in the nav/header.
- `homepage-layout`: Structure, content, and responsive behavior of the root (`/`) page — hero, research questions + section cards, bio + external link card, closing quote, burnt-orange dividers.

### Modified Capabilities

_None._ The existing `color-scheme` and `visual-dividers` specs already describe the palette and `<hr>` styling this change consumes; the new homepage reuses those rules rather than altering them.

## Impact

- **Config**: `config/_default/hugo.toml` (title), `config/_default/params.toml` (author name).
- **Content**: `content/_index.md` replaced with final copy and front matter for the custom layout.
- **Layouts**: new `layouts/index.html` (Congo override for the root page). No changes inside `themes/congo/`.
- **Assets**: new `static/images/duncan.jpg` placeholder photo.
- **Docs**: `IMPLEMENTATION_PLAN.md` rewritten to describe this change; site-wide "Duncan Poisson" mentions updated.
- **Preserved**: `layouts/partials/functions/warnings.html` stays as-is; `assets/css/custom.css` divider styling is reused, not replaced.
- **Build**: `hugo --minify` must exit 0 with zero ERROR lines.
