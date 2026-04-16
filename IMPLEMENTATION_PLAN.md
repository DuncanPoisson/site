# Implementation Plan — Design Overhaul

**Change:** `design-overhaul` ([openspec/changes/design-overhaul/](openspec/changes/design-overhaul/))
**Status:** Proposed — ready for bootstrap/build
**Owner:** Duncan Poisson

## Goal

Transform the site from Congo's default violet/fuchsia look into a cohesive personal brand with a warm cream, forest green, and burnt orange palette. Restructure navigation so the About page is the landing page, rename Research to Journal with a box-grid layout, add a Blog image+text list layout, introduce a new Projects portfolio section, and add burnt orange visual dividers.

## Scope

**In scope**
- Custom color scheme (`duncan.css`) with warm cream backgrounds, forest green primary, burnt orange secondary — light and dark modes.
- Favicon override: burnt orange square replacing purple.
- Navigation: About (→ `/`), Blog, Journal, CV, Projects. No separate Home item.
- Homepage = About page merged into root `_index.md`.
- Journal section: `content/research/` renamed to `content/journal/`, box-grid list layout (3/2/1 responsive columns).
- Blog list layout: horizontal rows with optional thumbnail image + title/date/description.
- Projects section: new portfolio showcase at `content/projects/` with card-grid layout.
- Visual dividers: burnt orange `<hr>` styling via `assets/css/custom.css`.

**Out of scope**
- Final content (all content remains placeholder).
- Custom single-page layouts (only list/landing layouts).
- JavaScript, animations, search, comments, analytics.
- Modifications to files inside `themes/congo/`.

## Approach

1. **Color scheme first.** Create `assets/css/schemes/duncan.css` with full CSS variable sets for light and dark modes. Update `params.toml`. Replace favicon.
2. **Navigation + homepage.** Restructure `menus.toml`. Merge about content into `_index.md`. Delete `content/about/`.
3. **Journal rename + layout.** `git mv content/research/ content/journal/`. Update all cross-link refs. Create `layouts/journal/list.html` with a CSS Grid box layout.
4. **Blog layout.** Create `layouts/blog/list.html` with flexbox image+text rows. Add thumbnail field to example post.
5. **Projects section.** Create content and layout for the new Projects portfolio.
6. **Visual polish.** Add `assets/css/custom.css` for divider styling. Add dividers to homepage.
7. **Validate.** `hugo --minify` exits 0, no ERRORs. Visual check on `hugo server -D`.

## Key Design Decisions

| Decision | Why |
|---|---|
| All customization via overrides, not theme edits | Theme stays updatable; `layouts/`, `assets/css/`, `config/` overrides are Hugo's intended mechanism. |
| Layout-specific CSS in `<style>` blocks within layout files | Co-locates markup and styles; `custom.css` reserved for global overrides. |
| No placeholder images for missing thumbnails | Avoids visual crutches that never get replaced. |
| `<hr>` styling instead of custom shortcode | Markdown `---` already produces `<hr>`; CSS is simpler than a shortcode. |
| Homepage = About via Congo's `page` layout | Already configured; just needs richer content in `_index.md`. |

## Deliverables

- `assets/css/schemes/duncan.css` — custom color scheme (light + dark)
- `assets/css/custom.css` — divider styling
- `static/favicon.png` — burnt orange favicon
- `config/_default/params.toml` — updated colorScheme
- `config/_default/menus.toml` — restructured navigation
- `content/_index.md` — merged homepage + about
- `content/journal/` — renamed from `content/research/`
- `content/projects/` — new portfolio section with example project
- `layouts/journal/list.html` — box-grid layout
- `layouts/blog/list.html` — image+text layout
- `layouts/projects/list.html` — card-grid layout
- All cross-links updated, build passes clean

## References

- [proposal.md](openspec/changes/design-overhaul/proposal.md) — why this change
- [design.md](openspec/changes/design-overhaul/design.md) — how it's structured
- [specs/](openspec/changes/design-overhaul/specs/) — delta specs per capability
- [tasks.md](openspec/changes/design-overhaul/tasks.md) — implementation checklist
