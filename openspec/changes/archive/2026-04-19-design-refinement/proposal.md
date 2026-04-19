## Why

The current design lands close to intent but three gaps remain: the color palette still reads light and slightly washed out (the primary green is too bright, the cream background blends with the header), the homepage's middle section is a single column of bulleted questions rather than a browsable narrative with real photos, and navigation exposes a CV tab that's less useful to visitors than a visual Gallery. This iteration tightens the palette, restructures the homepage's middle section around two real photos, and swaps CV for Gallery.

## What Changes

- **Color scheme**: darker forest green primary (`#082620` replaces `#429347`), warmer page background (`#ead4bb` replaces `#fff6ed`), distinct header/nav band (`#d1baa3`), near-black body text (`#0e1416`), and light-mint text inside bubbles (`#EAF9E1`). Burnt orange accent (`#C74D20`) is unchanged. Dark mode regenerated around the new primary.
- **Homepage section 2 "What is this Website?"** is rewritten as a two-row matrix: row 1 is text-left / photo-right (`photo1.png`), row 2 is photo-left / text-right (`photo2.png`). Existing bullet-list of research questions is replaced by two new body paragraphs (verbatim copy supplied). The three section bubbles (Blog / Journal / Projects) move below the matrix, with refined subtitle copy.
- **Homepage section 3 "Who am I?"**: the "More of Me" bubble is centered, widened to 50–60% of the container, text centered within, and its href updated to `https://linktr.ee/duncanpoisson` (replacing the TODO placeholder).
- **Bubble styling fixes**: remove `<li>` bullet dots on bubble lists, center bubble titles, left-align bubble descriptions, apply new primary-green background and light-mint text color to all bubbles.
- **Navigation**: remove `CV` menu item, add `Gallery` pointing to `/gallery/`. New order: About, Blog, Journal, Gallery, Projects. CV content files remain on disk (the `/cv/` URL continues to resolve).
- **New Gallery capability**: `/gallery/` is a grid of collection cards (three per row desktop, two tablet, one mobile) styled like the Projects list. Each collection is a subdirectory; clicking a collection opens a photo grid. One example collection is committed so Duncan can see the pattern.

## Capabilities

### New Capabilities

- `gallery`: Grid-of-collections landing at `/gallery/`, per-collection photo grid pages, collection metadata (title, description, cover image), custom `layouts/gallery/list.html`.

### Modified Capabilities

- `color-scheme`: Primary green shifts to `#082620`; neutral background shifts to `#ead4bb`; header/nav band color `#d1baa3` becomes a required distinct tone; body text `#0e1416` and bubble-text `#EAF9E1` are codified; dark palette is regenerated. Navigation-structure requirement updated (CV replaced by Gallery; new order: About, Blog, Journal, Gallery, Projects).
- `homepage-layout`: Section 2 "What is this Website?" requirement replaced with a two-row photo/text matrix plus refreshed bubble copy. Section 3 bubble centering + href update. Header-title and section-heading color values updated to the new primary green. Bubble visual style requirements codified (background `#082620`, text `#EAF9E1`, centered title, left-aligned description, no list markers).
- `site-identity`: Header-title color requirement updated from `#429347` to the new primary green `#082620` (still sourced via `assets/css/custom.css`, not a theme edit).

## Impact

- **Files modified**: `assets/css/schemes/duncan.css` (full palette regen), `assets/css/custom.css` (header title color, list-marker resets, nav/header band rule), `config/_default/menus.toml` (CV → Gallery), `content/_index.md` (section-2 copy + bubble subtitles), `layouts/index.html` (matrix layout + bubble CSS overhaul + centered wide More-of-Me), `openspec/specs/color-scheme/spec.md`, `openspec/specs/homepage-layout/spec.md`, `openspec/specs/site-identity/spec.md` (updated at archive time).
- **Files added**: `content/gallery/_index.md`, `content/gallery/<example-collection>/_index.md` plus at least one placeholder image, `layouts/gallery/list.html`, `layouts/gallery/single.html` (or equivalent to render a collection's photo grid).
- **Files untouched**: everything under `themes/congo/`; `layouts/partials/functions/warnings.html`; CV content at `content/cv/` (still resolves at `/cv/` even without a nav entry); `static/images/duncan.jpg`, `static/images/photo1.png`, `static/images/photo2.png` (already present).
- **Backpressure**: `hugo --minify` must exit `0` with no `ERROR` lines. Manual check: desktop + mobile, light + dark; all four nav items reach the correct page; Gallery landing shows the example collection card; the collection page opens to the photo grid.
- **Breaking-ish**: any direct links to `/cv/` still work; bookmarks to a prior nav order will reorder visually but not break.
