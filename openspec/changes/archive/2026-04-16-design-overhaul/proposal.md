## Why

The site currently uses Congo's default violet/fuchsia color scheme and stock layouts, making it visually generic. A custom color palette (warm cream, forest green, burnt orange), restructured navigation, and section-specific layouts will give the site a distinct, cohesive identity that reflects the author's academic aesthetic. The Research section is also being renamed to Journal and a new Projects portfolio section is being added.

## What Changes

- **Custom color scheme** (`duncan.css`): warm cream backgrounds, forest green (#429347) primary, burnt orange (#C74D20) secondary — replaces all default violet/fuchsia. Includes dark mode variant.
- **Favicon update**: colored square changes from purple to burnt orange.
- **Navigation restructure**: reordered to About (→ `/`), Blog, Journal, CV, Projects. "Home" nav item removed; About IS the homepage.
- **Homepage = About merge**: root `/` serves as both landing page and about page. Separate `content/about/` section removed; content folded into `content/_index.md`.
- **Journal section**: `content/research/` renamed to `content/journal/`. Custom box-grid list layout (3/2/1 columns responsive). All internal cross-links updated.
- **Blog list layout**: custom horizontal layout with thumbnail image on left, title/date/description on right. Responsive stacking on mobile.
- **Projects section** (new): portfolio showcase at `content/projects/`. Card/box-grid list layout. Individual project pages with status, tags, external links.
- **Visual dividers**: burnt orange `<hr>` styling via `assets/css/custom.css`, usable site-wide.

## Capabilities

### New Capabilities
- `color-scheme`: Custom Congo color scheme file, favicon override, and dark mode palette
- `journal-layout`: Box-grid list layout for the Journal section (renamed from Research)
- `blog-layout`: Horizontal image+text list layout for the Blog section
- `projects`: Portfolio showcase section with card layout and project pages
- `visual-dividers`: Burnt orange horizontal divider styling

### Modified Capabilities
- (no existing specs to modify — `openspec/specs/` is currently empty)

## Impact

- **Config**: `params.toml` (colorScheme), `menus.toml` (nav order/items), `hugo.toml` (no changes expected)
- **Content**: `content/research/` → `content/journal/`, `content/about/` removed, `content/_index.md` updated, `content/projects/` created
- **Assets**: new `assets/css/schemes/duncan.css`, new `assets/css/custom.css`
- **Layouts**: new `layouts/journal/list.html`, new `layouts/blog/list.html`, new `layouts/projects/list.html`
- **Cross-links**: all `ref "/research/..."` updated to `ref "/journal/..."`
- **Favicon**: new `static/favicon.png` or equivalent override
- **No theme modifications**: all changes via config/layout/asset overrides
