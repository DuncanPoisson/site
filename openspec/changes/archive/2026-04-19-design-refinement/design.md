## Context

The site shipped with a custom palette (`assets/css/schemes/duncan.css`) and a custom root layout (`layouts/index.html`) two changes ago (`design-overhaul`, `custom-homepage`). Both are working, but the owner's feedback after seeing it live is:

- The cream background and bright green read "garden" rather than "forest"; he wants a deeper, more grounded forest primary and a warmer, tanner page.
- The header/nav bar currently blends into the page because both use the same neutral; the header should be its own band.
- The research-questions list in section 2 is text-heavy and leaves the two photos (`photo1.png`, `photo2.png`) — already shipped to `static/images/` — with nowhere to live.
- A CV nav entry is premature; a Gallery of photo collections is a better fit for the site's current scope.
- The bubble `<ul>` still renders default list markers; bubble titles should center while descriptions stay left-aligned.

All customization must stay outside `themes/congo/`. `layouts/partials/functions/warnings.html` must remain untouched (prior local override). Three image assets are already in place: `static/images/{duncan,photo1,photo2}.jpg|png`.

## Goals / Non-Goals

**Goals**
- Swap every hard-coded `#429347` and `#fff6ed` reference in project-owned CSS and templates for the new primary/background tones, driven through the `duncan.css` scheme file where possible and through minimal `custom.css` rules where the theme doesn't read a variable.
- Rebuild homepage section 2 as a two-row CSS grid with the owner-authored copy and the two photos.
- Reshape the "More of Me" bubble into a centered, wider pill with centered text; point it at the real Linktree URL.
- Introduce a `gallery` content section, a list layout that renders collection cards, and a single layout that renders the photos inside a collection — mirroring the existing `projects/list.html` card pattern so visuals stay consistent.
- Keep `/cv/` alive as a reachable-but-unlinked URL.

**Non-Goals**
- Redesigning the Blog, Journal, or Projects list layouts (out of scope; bubble copy on the homepage changes but those section pages do not).
- Replacing placeholder gallery photos with final imagery — Duncan will drop those in later.
- Image optimization pipeline work (Hugo image resources, srcset, etc.) — defer until the gallery is actually populated.
- Modifying `themes/congo/` or editing `warnings.html`.
- Creating any new top-level nav items beyond Gallery.

## Decisions

### Palette is driven through the scheme file, not per-element overrides

Congo reads `--color-neutral-*`, `--color-primary-*`, `--color-secondary-*` as RGB triplets. Re-deriving the full 50→950 scale for primary around `#082620` and for neutral around `#ead4bb` means most theme surfaces (links, buttons, tag pills, footer, typography color) pick up the new tones automatically. Only two surfaces need explicit rules in `custom.css`: the header/nav band (Congo's header partial uses `bg-neutral` or similar, which we override by selector) and the site-title color (already in place; value shifts to `#082620`).

*Alternative considered*: per-component overrides in `custom.css`. Rejected — it guarantees the palette drifts out of sync whenever a new page type is added.

### Header/nav band uses a distinct neutral (`#d1baa3`)

The new page background `#ead4bb` and the header band `#d1baa3` are close enough to harmonize but distinct enough that the nav has a visible boundary without needing a border. Implement via a `custom.css` rule on Congo's header element (observed selector: `header.sticky` / `nav`; confirm at build time and pick the narrowest selector that binds both light and dark modes).

### Section 2 matrix uses CSS grid with a 2fr/3fr split

Two rows, alternating orientation. Desktop: `grid-template-columns: 2fr 3fr` for row 1 and `3fr 2fr` for row 2, giving each photo the slightly larger share (photos carry more information than the copy blocks). Mobile (≤768px): collapse each row to a single column with the order specified by source order (text then photo for row 1, photo then text for row 2). Inline `<style>` in `layouts/index.html` is the established precedent.

*Alternative considered*: Flexbox with `flex-direction: row-reverse` on row 2. Rejected — grid gives cleaner column-width control and simpler responsive collapse.

### "More of Me" bubble: reuse `.home-card` class, add a `.home-card-wide` modifier

Keeps the visual identity aligned with the three Blog/Journal/Projects cards while unlocking a wider, centered layout. The `<ul>` wrapper gets `list-style: none` globally (applies to both card groups).

### Gallery list layout mirrors `projects/list.html`

Same card pattern (rounded rectangle, cream background with burnt-orange border, hover lift). A collection is a page bundle: `content/gallery/<slug>/_index.md` carries `title`, `description`, and an optional `cover` image path. Hugo's `.Pages` iteration at `/gallery/` yields the collections; `.RegularPages` inside a collection yields the images only if we choose to model photos as content files. Simpler: treat images as `page.Resources` on the collection's `_index.md` and iterate `.Resources.ByType "image"` in `layouts/gallery/single.html`.

*Alternative considered*: one markdown file per photo. Rejected — adds content overhead for every new image with no benefit over page resources.

### Example collection ships with one placeholder image so the layout isn't empty

Reuse `static/images/example-thumb.png` (already on disk) or drop it into the collection bundle. Duncan replaces it when he has real photos.

### CV stays reachable but unlinked

Only `config/_default/menus.toml` changes — `content/cv/_index.md` is untouched. `/cv/` continues to render.

## Risks / Trade-offs

- **Congo header selector may change.** Mitigation: validate the selector against the built `public/` output before committing; if fragile, override `layouts/partials/header.html` rather than relying on CSS. Keep the override minimal.
- **Dark-mode regen may produce awkward contrast around the very-dark primary (`#082620`).** Mitigation: in dark mode, slide the primary scale so the 500 is a brighter forest green (around `#3a7a50`) rather than the same `#082620` — matching what the existing `duncan.css` already does for the old primary. Verify contrast for links and headings against the warm-dark neutral.
- **Bubble background `#082620` + text `#EAF9E1` contrast.** Contrast ratio ≈ 15:1 — well above WCAG AA. No risk.
- **Page background `#ead4bb` vs. body text `#0e1416`.** Contrast ≈ 16:1. No risk.
- **Header band `#d1baa3` vs. nav text.** Primary-colored nav items on this band need checking; the green `#082620` on `#d1baa3` computes to ~11:1, fine. Neutral/dark nav items also fine.
- **Gallery collection with zero images.** Mitigation: `layouts/gallery/single.html` renders an empty-state message ("No photos yet.") so a freshly-scaffolded collection doesn't show a broken grid.
- **`hugo --minify` warnings from `warnings.html` partial.** Not a risk — that partial is an existing local override we must not touch; any warnings it emits are informational, not `ERROR` lines.

## Migration Plan

Linear; one commit per ticket:

1. **Palette regen** — rewrite `assets/css/schemes/duncan.css` around new primary/neutral; leave secondary alone. Update `custom.css` header-title rule to `#082620`. Add header-band rule.
2. **Homepage CSS + layout** — rewrite the inline `<style>` block and markup in `layouts/index.html` for the matrix + wider centered More-of-Me + bubble styling (background, text color, centered title, list-marker reset).
3. **Homepage copy** — update `content/_index.md` params: remove `questions_body`, add `section2_row1_text` / `section2_row2_text` (or inline in the layout if simpler), refresh bubble subtitles, keep hero and bio verbatim.
4. **Menu swap** — edit `config/_default/menus.toml`: remove CV, insert Gallery at weight 40, bump Projects to 50.
5. **Gallery capability** — create `content/gallery/_index.md`, one example collection bundle, `layouts/gallery/list.html`, `layouts/gallery/single.html`.
6. **Validate** — run `hugo --minify`, check for `ERROR` lines, spot-check in browser (desktop + mobile, light + dark).

Rollback: revert the change's commits on the branch; no data migration, no external state.

## Open Questions

- None at spec time. Owner-supplied copy is final; owner-supplied URL (`https://linktr.ee/duncanpoisson`) is final; example-collection content is a placeholder by design.
