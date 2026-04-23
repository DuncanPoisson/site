## 1. Font and styling foundation

- [ ] 1.1 Vendor Caveat (400 + 600 weights, woff2) under `static/fonts/caveat/` with the OFL license file alongside.
- [ ] 1.2 Add an `@font-face` block for Caveat in `assets/css/custom.css` with `font-display: swap`.
- [ ] 1.3 Add base CSS rules for `.marginalia-grid`, `.marginalia-body`, `.marginalia-rail`, `.marginalia`, `.marginalia--desktop`, `.marginalia--mobile`, `.marginalia__date`, and `.marginalia__note` in `custom.css` covering: desktop two-column grid (article + rail) at `lg:` breakpoint; mobile inline `<details>` styling; sticky-note background derived from the secondary scale; deterministic per-note rotation via a `--marginalia-rot` custom property; print stylesheet hiding the rail and showing inline notes expanded.

## 2. Single-page layout override

- [ ] 2.1 Create `layouts/_default/single.html` as a verbatim copy of `themes/congo/layouts/single.html`, then replace the `{{ .Content | emojify }}` block with the marginalia-injecting wrapper from `design.md`.
- [ ] 2.2 Gate marginalia rendering on `(or (eq .Section "blog") (eq .Section "journal"))` AND a non-empty `.Params.marginalia`.
- [ ] 2.3 Implement the paragraph-walking split-on-`</p>` logic, dual-render (rail + inline) emission, deterministic rotation injection, and out-of-range `warnf`.
- [ ] 2.4 Mark the desktop rail with `aria-hidden="true"`.

## 3. Example content

- [ ] 3.1 Add 2-3 example marginalia entries to the existing example blog post (`content/blog/<example>.md`) covering different paragraphs and dates.
- [ ] 3.2 Add 2-3 example marginalia entries to the existing example journal entry under `content/journal/<project>/<entry>.md`.

## 4. Validation

- [ ] 4.1 Run `hugo --minify`; verify exit 0 and zero `ERROR` lines, and verify the out-of-range `warnf` only fires on intentionally malformed examples (none if no malformed examples).
- [ ] 4.2 Manual walk-through: load both example pages in `hugo server` at desktop (≥ 1024px) and mobile (< 1024px) widths in light and dark mode; verify rail alignment, mobile `<details>` collapse, deterministic rotation, no font flash, and that pages without marginalia render unchanged.
- [ ] 4.3 Print-preview both example pages; verify rail is hidden and notes appear inline expanded.
- [ ] 4.4 Inspect built page network panel; verify zero requests to fonts.googleapis.com or fonts.gstatic.com.
- [ ] 4.5 Run `npx openspec validate add-marginalia --strict`.
