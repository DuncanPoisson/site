## 1. Palette + header band

- [ ] 1.1 Rewrite `assets/css/schemes/duncan.css` light-mode block — regenerate `--color-primary-*` around `#082620` (50→950), regenerate `--color-neutral-*` around `#ead4bb` with the deep end sliding toward `#0e1416`, leave `--color-secondary-*` (burnt orange) unchanged.
- [ ] 1.2 Rewrite `assets/css/schemes/duncan.css` dark-mode block — regenerate primary and neutral scales for dark backgrounds against the new primary `#082620`; verify WCAG AA contrast for headings and links.
- [ ] 1.3 Update `assets/css/custom.css` — change header site-title color value to `#082620` (keep the `rel="me"` selector), keep the burnt-orange `<hr>` rule intact, add a new rule that sets the header/nav band background to `#d1baa3`. Validate the selector against Congo's rendered header.

## 2. Homepage section-2 matrix + bubble styling

- [ ] 2.1 Update `layouts/index.html` inline `<style>` — add `.home-matrix` grid rules (row 1 `2fr 3fr`, row 2 `3fr 2fr`, gap, image sizing), refresh `.home-card` (background `#082620`, color `#EAF9E1`, centered title, left-aligned description), add `.home-card-wide` modifier for the centered 50–60% More-of-Me card, add `list-style: none` reset on `.home-cards` / `.home-cards-single` / any `<ul>` wrapping bubbles, update all existing primary-green references to `#082620`.
- [ ] 2.2 Update `layouts/index.html` markup — change section-2 heading text to `What is this Website?` (capital W), replace the existing `{{ .Params.questions_body | markdownify }}` block with two matrix rows (row 1: text-left / `photo1.png` right; row 2: `photo2.png` left / text-right), keep the three bubbles but move them below the matrix, update bubble subtitles to the new copy, update the More-of-Me card to use `href="https://linktr.ee/duncanpoisson"` and the `.home-card-wide` modifier.
- [ ] 2.3 Update `content/_index.md` — remove the `questions_body` param, add two new params (or inline into `layouts/index.html`) carrying the two matrix-row paragraphs verbatim; keep `hero_intro` and `bio_body` byte-for-byte identical to the current file.

## 3. Navigation swap (CV → Gallery)

- [ ] 3.1 Edit `config/_default/menus.toml` — remove the `CV` entry, insert a `Gallery` entry (`pageRef = "gallery"`, `weight = 40`), bump `Projects` to `weight = 50`. Leave About / Blog / Journal weights unchanged.
- [ ] 3.2 Verify `content/cv/_index.md` is unchanged on disk and that `/cv/` still resolves in a local `hugo server` build.

## 4. Gallery capability

- [ ] 4.1 Create `content/gallery/_index.md` with front matter (`title = "Gallery"`, short `description`) — the landing page for the gallery section.
- [ ] 4.2 Create one example collection as a page bundle: `content/gallery/<example-slug>/_index.md` with `title`, `description`, optional `cover` front-matter; include at least one placeholder image file inside the bundle directory (reuse `static/images/example-thumb.png` or similar) so the collection page renders a non-empty grid.
- [ ] 4.3 Create `layouts/gallery/list.html` — renders a responsive grid of collection cards (3 cols ≥1024px, 2 cols ≥640px, 1 col <640px) using the same card pattern as `layouts/projects/list.html`.
- [ ] 4.4 Create `layouts/gallery/single.html` — renders the collection title, description, and a responsive grid of image resources from `.Resources.ByType "image"`; shows an empty-state message ("No photos yet.") when the collection has zero image resources.

## 5. Validation

- [ ] 5.1 Run `hugo --minify` — exit code `0`, zero `ERROR` lines. Investigate and fix any errors before proceeding.
- [ ] 5.2 Run `hugo server` and visually verify at localhost: (a) homepage section 1 unchanged; (b) section 2 shows the matrix on desktop and stacks correctly on a ≤768px viewport; (c) bubbles have no list markers, centered titles, left-aligned descriptions, new palette; (d) section-3 More-of-Me bubble is centered and 50–60% width; (e) nav shows About / Blog / Journal / Gallery / Projects in that order; (f) `/gallery/` renders the example collection card; (g) clicking the collection opens the photo grid; (h) `/cv/` still resolves; (i) header band color is visually distinct from page background; (j) dark mode looks coherent.
- [ ] 5.3 Verify `themes/congo/` has zero modifications and `layouts/partials/functions/warnings.html` is untouched (diff vs. `main`).
