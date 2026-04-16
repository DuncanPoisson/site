## Context

Congo's default `page` layout renders the root `/` using the merged `content/_index.md` content — fine for placeholder text but insufficient for a visually structured landing page with a photo, clickable section cards, and a closing quote. The design-overhaul change already shipped the color scheme (`assets/css/schemes/duncan.css`), burnt orange divider rules (`assets/css/custom.css`), section-specific list layouts (blog/journal/projects), and the "About → `/`" navigation.

This change adds the final piece: a bespoke homepage template and the real owner-authored copy, plus a site-wide rename from "Duncan Poisson" to "Duncan Xavier Haddock".

## Goals / Non-Goals

**Goals:**
- A responsive custom landing page with four clearly dividered sections.
- Three section-card links (Blog / Journal / Projects) with subtitles, consistently styled with a fourth "More of me" external card.
- Final copy rendered exactly as specified, including the Angela Davis closing quote.
- Canonical site name "Duncan Xavier Haddock" used in config, header, author metadata, and homepage.
- `hugo --minify` continues to build cleanly with no ERROR lines.

**Non-Goals:**
- Redesigning other layouts (blog / journal / projects list pages remain as-is).
- Editing `themes/congo/` internals — all overrides live in `layouts/`, `config/_default/`, `content/`, `static/`, and `assets/css/`.
- Swapping the actual photo at `static/images/duncan.jpg` (user replaces it later — a placeholder file is created).
- Picking a real Linktree URL (placeholder `https://linktr.ee/` — user updates later).
- Changing the existing nav ordering or menu items.

## Decisions

### D1. Override path: `layouts/index.html` over `layouts/_default/home.html`

Hugo's template lookup for the home page prefers `layouts/index.html`. Congo ships `layouts/_default/home.html` inside the theme; overriding at the root `layouts/index.html` level is the narrowest, least-coupled way to take over just the home page without redefining `_default/home.html` and inheriting its assumptions.

Alternative considered: `layouts/_default/home.html` override — would shadow more of the theme's page-layout behavior unnecessarily. Rejected.

### D2. Single template file with inline `<style>` block

Per design-overhaul precedent ("Layout-specific CSS in `<style>` blocks within layout files"), the homepage styles are co-located in `layouts/index.html`. Global rules (dividers, color variables) remain in `assets/css/custom.css` and `assets/css/schemes/duncan.css`.

Alternative considered: separate `assets/css/home.css` — adds a bundled asset and a pipeline step for ~80 lines of scoped CSS. Rejected.

### D3. Section cards rendered as `<a>` anchors styled as blocks

Semantic clickable cards are `<a class="home-card">` elements with block-level display. This gives the whole rectangle a single hit target (accessibility win), keyboard focus by default, and no JS needed.

Alternative considered: `<div>` wrapping text + link with JS click handler — worse for a11y and adds JS for no benefit. Rejected.

### D4. Responsive layout via CSS Grid + media query breakpoint at 768px

- **Hero**: `display: grid; grid-template-columns: minmax(0, 280px) 1fr; gap: 2rem;` → collapses to single column at `≤768px` with photo above text.
- **Card row**: `grid-template-columns: repeat(3, 1fr);` → single column at `≤768px`.

768px matches Congo's default breakpoint convention and standard tablet-portrait split.

### D5. Burnt orange divider reuses existing `<hr>` style

The `visual-dividers` spec already styles `<hr>` as 2px burnt orange at 0.8 opacity via `assets/css/custom.css`. The new layout uses plain `<hr>` tags — no per-section override needed. This keeps `visual-dividers` as the single source of truth for the rule.

### D6. Site title color set via partial override, not theme fork

The spec says the header/nav site title should display in primary green `#429347`. Congo renders the header title via `themes/congo/layouts/partials/header/basic.html`. We override that partial at `layouts/partials/header/basic.html` (or add a targeted CSS rule in `custom.css` keyed on the header title selector — whichever preserves Congo's markup more faithfully).

Decision: **prefer CSS-only** — add a rule like `.site-title, header a.logo { color: var(--color-primary-500); }` (exact selector verified at implementation time against the built HTML). Avoids copying and drifting from Congo's partial.

Alternative considered: copying `basic.html` into `layouts/partials/header/basic.html`. Rejected as higher maintenance; only falls back to this if CSS selectors prove brittle.

### D7. Photo placeholder strategy

Create `static/images/duncan.jpg` as a tiny placeholder file (a 1×1 or small stock JPG) committed to the repo, so the build succeeds and the `<img>` has a valid target. Document in the change's tasks that the user will overwrite this with a real photo.

### D8. Linktree URL as a literal placeholder

`https://linktr.ee/` is committed as the literal href. A task entry and an inline HTML comment mark it as TODO so the user can grep for it later.

### D9. Homepage front matter turns off Congo article chrome

`content/_index.md` front matter sets `showDate = false`, `showReadingTime = false`, and similar to prevent Congo from rendering a date line / reading-time block above the custom layout. If the custom `layouts/index.html` fully controls the `<main>` content, most of this is moot — but the front matter is set defensively.

## Risks / Trade-offs

- **Risk**: Congo's default `home.html` output conflicts with a top-level `layouts/index.html` override. → **Mitigation**: verify with `hugo --minify` and browser check that `/` renders the new template; fall back to `layouts/_default/home.html` override if needed.
- **Risk**: Header green-title CSS selector is fragile if Congo renames its classes during a theme update. → **Mitigation**: target a semantic selector (`header a[href="/"]`, `.site-title`) rather than a utility class; add a comment in `custom.css` pointing to the spec; the site pins Congo via submodule so updates are deliberate.
- **Risk**: Bulky hero image hurts first paint. → **Mitigation**: placeholder is tiny; instruct the user (in tasks) to export the replacement at ~800×800 JPG, lazy-loaded via the existing `enableImageLazyLoading` param.
- **Risk**: "Duncan Poisson" lingering references cause inconsistency. → **Mitigation**: grep-based search across config/content/docs is part of the tasks; `IMPLEMENTATION_PLAN.md` is rewritten so it does not perpetuate the old name.

## Migration Plan

1. Rename config + content references in one commit (title, author, `_index.md`, plan doc).
2. Add the custom homepage layout, inline styles, and divider HTML.
3. Add the placeholder photo asset.
4. Apply the green-header-title CSS.
5. Build with `hugo --minify`, confirm exit 0 and no ERROR lines.
6. Visually inspect at `hugo server` → `/`, both light and dark modes, desktop + mobile widths.

Rollback: `git revert` the merged range on `about-section`; no external systems, DBs, or CI state involved.

## Open Questions

- None blocking. The placeholder photo and Linktree URL are explicitly temporary; the user controls the swap-in.
