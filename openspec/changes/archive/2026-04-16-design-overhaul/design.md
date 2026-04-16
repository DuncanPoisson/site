## Context

The site runs Hugo with the Congo theme (git submodule at `themes/congo/`). Congo supports customization through:
- **Color schemes**: CSS files in `assets/css/schemes/<name>.css` defining CSS custom properties (RGB triplets). Set via `colorScheme` in `params.toml`.
- **Custom CSS**: `assets/css/custom.css` is automatically loaded if present.
- **Layout overrides**: Hugo's lookup order means `layouts/<section>/list.html` overrides the theme's default list template for that section.
- **Favicon**: `static/favicon.png` (or SVG) overrides the theme default.

Current state: default `congo` scheme (violet primary, fuchsia secondary), five nav items (Home, Blog, Research, CV, About), separate about page, stock list layouts everywhere.

Constraint: `layouts/partials/functions/warnings.html` exists as a local override — must not be removed.

## Goals / Non-Goals

**Goals:**
- Distinct visual identity via custom color scheme (warm cream, forest green, burnt orange)
- Intuitive navigation with About as the landing page
- Section-specific layouts: box-grid for Journal and Projects, image+text rows for Blog
- Responsive design (mobile-first) for all custom layouts
- All customization via Hugo's override mechanisms — zero theme modifications

**Non-Goals:**
- Custom Hugo shortcodes beyond what's needed for dividers
- JavaScript-based interactivity or animations
- Search, comments, analytics, or newsletter functionality
- Final content — all content remains placeholder
- Custom single-page layouts (only list layouts are customized)

## Decisions

### 1. Color scheme: single CSS file with full variable set

**Decision:** Create `assets/css/schemes/duncan.css` with all `--color-neutral-*`, `--color-primary-*`, and `--color-secondary-*` variables in RGB triplet format matching Congo's convention.

**Why:** Congo's scheme system expects a complete variable set. Partial overrides risk falling through to hardcoded defaults or causing contrast issues. The full set ensures consistent rendering in both light and dark modes.

**Alternatives considered:**
- Using `custom.css` for color overrides → fragile, doesn't integrate with Congo's appearance switcher.
- Forking an existing scheme (e.g., `fire.css`) → still need to change every value; starting fresh is clearer.

### 2. Navigation: About at root, no separate Home item

**Decision:** The `About` menu item points to `/` (weight 10). No separate `Home` item. `content/about/` is deleted; its content merges into `content/_index.md`.

**Why:** Having both Home and About as separate pages creates a redundant click. The site is personal — the landing page *is* the about page. Congo's `page` homepage layout already renders `_index.md` as a content page, which is exactly what we want.

### 3. Journal rename: content directory + layout override

**Decision:** `git mv content/research/ content/journal/`. Create `layouts/journal/list.html` for the box-grid. Update `ref` shortcodes in all content files.

**Why:** Hugo resolves section names from the content directory path. Renaming the directory is the canonical way to rename a section. The layout override uses Hugo's lookup order: `layouts/<section>/list.html` takes priority over `themes/congo/layouts/_default/list.html`.

### 4. Custom list layouts: inline HTML+CSS in layout templates

**Decision:** Each custom layout (`layouts/journal/list.html`, `layouts/blog/list.html`, `layouts/projects/list.html`) contains its own `<style>` block with scoped CSS, plus HTML that uses Congo's base template (`{{ define "main" }}`).

**Why over external CSS:** Layout-specific styles are co-located with their markup, making each layout self-contained. `custom.css` is reserved for site-wide overrides (dividers, global tweaks). This avoids CSS specificity conflicts.

**Why not partials:** These layouts are distinct enough that shared partials would add indirection without meaningful reuse. If the grid pattern repeats across Journal and Projects, a shared partial can be extracted later.

### 5. Blog image support: `thumbnail` frontmatter field

**Decision:** Blog list layout checks for `.Params.thumbnail` in each page's frontmatter. If present, renders the image at ~180px wide on the left. If absent, renders text-only (no placeholder image).

**Why:** Simpler than requiring a `feature` image or page bundle. The `thumbnail` field is a relative path to an image in `static/` or a page resource. No placeholder image avoids creating a visual crutch that never gets replaced.

### 6. Favicon: static PNG override

**Decision:** Generate a simple square PNG (`static/favicon.png`) with `#C74D20` burnt orange background. Congo's base template references `/favicon.png` if it exists.

**Why:** PNG is universally supported and trivial to generate. SVG would be more flexible but Congo's default template expects PNG.

### 7. Visual dividers: CSS-only via custom.css

**Decision:** Style `<hr>` elements globally in `assets/css/custom.css` with burnt orange color, 2px solid, and appropriate margin. No shortcode needed — standard Markdown `---` produces `<hr>`.

**Why:** A shortcode would be over-engineering. Markdown already has `---` for horizontal rules; styling the resulting `<hr>` is the simplest approach.

## Risks / Trade-offs

- **Color scale generation accuracy** → Manually generating 50–950 scales risks poor contrast at extremes. Mitigation: test with Congo's built-in elements (buttons, tags, code blocks) and verify WCAG AA contrast for text on primary/secondary backgrounds.
- **Dark mode coherence** → Inverting a warm palette can feel muddy. Mitigation: use a warm dark background (e.g., `#1a1410`) rather than pure black; keep green and orange as-is since they're already high-saturation.
- **Layout override fragility** → Congo theme updates could change the base template structure that layouts extend. Mitigation: layouts use only `{{ define "main" }}` which is stable in Congo. Pin the theme submodule to a known commit.
- **Cross-link breakage during rename** → `ref "/research/..."` will fail after the directory rename. Mitigation: update all refs in the same commit as the rename; `hugo --minify` validates all refs at build time.

## File Map

| File | Action | Purpose |
|------|--------|---------|
| `assets/css/schemes/duncan.css` | Create | Custom color scheme |
| `assets/css/custom.css` | Create | Divider styling, global overrides |
| `config/_default/params.toml` | Modify | Set `colorScheme = "duncan"` |
| `config/_default/menus.toml` | Modify | Restructure nav |
| `content/_index.md` | Modify | Merge about content, add hero |
| `content/about/` | Delete | Folded into root `_index.md` |
| `content/research/` → `content/journal/` | Rename | Section rename |
| `content/projects/_index.md` | Create | Projects landing page |
| `content/projects/example-project.md` | Create | Example project |
| `content/blog/example-post.md` | Modify | Add `thumbnail` field |
| `layouts/journal/list.html` | Create | Box-grid layout |
| `layouts/blog/list.html` | Create | Image+text layout |
| `layouts/projects/list.html` | Create | Card-grid layout |
| `static/favicon.png` | Create | Burnt orange favicon |
