# Implementation Plan — Marginalia

**Change:** `add-marginalia` ([openspec/changes/add-marginalia/](openspec/changes/add-marginalia/))
**Status:** Proposed — ready for bootstrap/build
**Owner:** Duncan Xavier Haddock
**Branch:** `ralph/my-feature`

## Goal

Make every long-form post a living organism. Each blog post and journal entry grows a margin of dated, handwritten-style notes that future-Duncan pins to specific paragraphs over time — Post-Its from the future on the past. A 2026 post might, by 2028, carry three small notes in its right gutter saying *"disagree now,"* *"this aged well,"* or *"see also journal/2027-11-03."* It's an old scholarly gesture (marginalia) turned inward, and it makes the site read less like a bulletin board and more like a working notebook.

## Scope

**In scope**
- New `marginalia` capability authored entirely in front-matter:
  ```yaml
  marginalia:
    - paragraph: 3
      date: 2027-11-03
      note: "I'd put this differently now."
  ```
  Required fields: `paragraph` (1-indexed int), `date` (ISO date), `note` (markdown).
- Project-level override at `layouts/_default/single.html` (a copy of Congo's `single.html` with the `.Content` block wrapped) that:
  - Splits rendered HTML on `</p>` boundaries.
  - Walks paragraphs in order, injecting marginalia after the Nth `</p>`.
  - Dual-renders each note: once as a mobile-inline `<details>` (canonical, screen-reader visible), once in a desktop right rail (visual decoration, `aria-hidden="true"`).
- New CSS in `assets/css/custom.css`: a two-column grid at `lg:` breakpoint (article body + rail), sticky-note styling, deterministic per-note rotation in [-2deg, +2deg] from a date hash, mobile `<details>` styling, print stylesheet that shows the inline copy and hides the rail.
- Self-hosted **Caveat** webfont (Google OFL) under `static/fonts/caveat/` — no third-party font requests at runtime.
- Scope gated to `Section in {blog, journal}` via the layout — Gallery, About, CV, Homepage are explicitly ignored.
- Example marginalia shipped on the existing example blog post and example journal entry so the feature is observable on a fresh build.
- Spec deltas: new `marginalia` spec; ADDED requirements on `blog-layout` and `journal-layout` capturing the new render expectations.

**Out of scope**
- Authoring UI / CMS / in-browser editing.
- Anchor-based targeting (`marginalia: [{anchor: "...", ...}]`) — designed-for in v2 but not built in v1.
- Marginalia on non-article pages (index, section landings, About, Gallery, CV, project landings).
- Inter-post backlinks rendered as marginalia.
- Any animation, scroll-triggered reveals, or JS dependencies.
- Modifying anything under `themes/congo/`.

## Approach

1. **Foundation first** — vendor the font, declare `@font-face`, write the base CSS rules covering desktop grid, mobile `<details>`, sticky-note styling, deterministic rotation via a `--marginalia-rot` custom property, and print stylesheet.
2. **Layout override** — copy Congo's `single.html` to `layouts/_default/single.html` and replace the `.Content` block with the marginalia-injecting wrapper. Gate on `Section` ∈ `{blog, journal}` AND non-empty `.Params.marginalia`. Implement paragraph-walking, dual-render emission, deterministic rotation injection, and a `warnf` for out-of-range paragraph indices.
3. **Examples** — add 2-3 marginalia entries to the existing example blog post and journal entry covering different paragraphs and dates.
4. **Validate** — `hugo --minify` exits 0; manual walk-through at desktop + mobile, light + dark, print preview; verify zero third-party font requests; `npx openspec validate add-marginalia --strict`.

## Key Design Decisions

| Decision | Why |
|---|---|
| Paragraph-index targeting (Nth `</p>`), not anchors | Zero markup burden on the author. Drop a note in front-matter; it shows up. Trade-off: editing the post body shifts indices — accepted, since marginalia naturally belong on stable, no-longer-being-edited posts. |
| Dual-render (rail + inline) with `aria-hidden` on the rail | Cannot statically position the rail to align with paragraphs without per-paragraph offsets. Two render passes keeps the feature CSS-only and Hugo-static-friendly. The mobile inline copy is canonical for accessibility; the rail is visual decoration. |
| Single override at `layouts/_default/single.html`, not per-section | The override only diverges from Congo's stock `single.html` in one place. One template avoids drift between near-identical files. Section gating happens inside the template via `.Section`. |
| Self-hosted Caveat (Google OFL), not a CDN webfont | No third-party requests at runtime — privacy-clean and offline-buildable. ~40KB across two weights — acceptable. |
| Deterministic rotation from a stable date hash | Each note rotates a small amount so they don't sit perfectly square — but the same date always produces the same angle, so there's no visual "shimmer" across rebuilds. |
| Scope to blog + journal only in v1 | Gallery is photo-led, About / CV are layout-led, homepage is layout-led. The capability spec is written so a v2 can extend scope by relaxing one requirement clause. |
| Caveat over Kalam / Architects Daughter | Legible at small sizes, two weights, well-paired with the existing serif body. |
| Theme files untouched | `themes/congo/` is off-limits per `AGENTS.md`. All work lives in `layouts/`, `assets/css/custom.css`, `static/`, and content front-matter. |

## Deliverables

- `static/fonts/caveat/{Caveat-Regular.woff2,Caveat-SemiBold.woff2,OFL.txt}` vendored.
- `assets/css/custom.css` extended with `@font-face` for Caveat and the full marginalia rule set (grid, rail, sticky-note, mobile inline, print).
- `layouts/_default/single.html` created as Congo-`single.html`-with-marginalia-wrapper.
- 2-3 example marginalia entries on the example blog post (`content/blog/<example>.md`).
- 2-3 example marginalia entries on the example journal entry (`content/journal/<project>/<entry>.md`).
- `hugo --minify` passes with zero `ERROR` lines on the build.
- `npx openspec validate add-marginalia --strict` passes (already does for the proposed artifacts; will re-run before archive).

## Future (v2+) — explicitly out of scope here

- **Anchor-based targeting**: `marginalia: [{anchor: "thesis-claim", date, note}]`, with body-side `{{< anchor "thesis-claim" >}}` shortcode. Survives paragraph-index drift on edits.
- **Note kinds**: `kind: correction | amplification | retraction`, each rendering with a distinct color/icon variant.
- **Scope expansion**: About page, project landings, perhaps Homepage.
- **Inter-post backlinks**: render "post X at /blog/.../#para-3 cites this paragraph" as automatically-generated marginalia.
- **Viewport-windowing**: only show notes whose target paragraph is currently in viewport (requires JS — declined for v1).
