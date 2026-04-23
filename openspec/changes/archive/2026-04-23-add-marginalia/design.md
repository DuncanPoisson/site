## Context

Congo's `single.html` (in `themes/congo/layouts/single.html:45`) renders the article body with `{{ .Content | emojify }}` inside a `max-w-prose` column. There's no per-paragraph hook in the theme, and Hugo's render-hook system covers headings, links, and images — not paragraphs. So if we want notes pinned to specific paragraphs, we either (a) post-process `.Content` after Hugo has rendered the markdown, or (b) require authors to mark anchors in the body markup.

Duncan's existing post surfaces are blog posts (`content/blog/*.md`, served via Congo's default `single.html`) and journal entries (`content/journal/<project>/*.md`, same). Both currently use Congo's article layout untouched; we have not overridden `single.html` at the project level yet. There are no existing render-hook overrides under `layouts/_default/_markup/` either — the design-refinement work touched `index.html`, section list templates, and CSS, but never the article body.

The site's theme files under `themes/congo/` are off-limits per `AGENTS.md`. So whatever we do has to live in `layouts/`, `assets/css/custom.css`, `static/`, and content front-matter.

## Goals / Non-Goals

**Goals:**
- Author marginalia entirely in front-matter — no shortcodes, no body-markup discipline.
- Render desktop notes in a right-side gutter aligned roughly with their target paragraph; render mobile notes inline-after-paragraph as collapsible `<details>`.
- Zero changes under `themes/congo/`.
- No runtime JS dependency for the base feature. CSS-only positioning and `<details>` for mobile collapse.
- No external network calls — vendor the handwriting font locally.
- The feature degrades cleanly: a post with no `marginalia:` front-matter renders identically to today.
- Accessibility: notes are reachable in DOM order, screen-reader-readable, do not visually overlap body text on any viewport.

**Non-Goals:**
- An authoring UI, CMS integration, or any in-browser editing.
- Marginalia on non-article pages (index, section landing, About, Gallery, CV).
- Anchor-based targeting (`marginalia: [{anchor: "thesis", ...}]`) — designed-for in v2 but not built in v1.
- Inter-post backlinks rendered as marginalia (e.g., "post X cites this paragraph"). v2 territory.
- Animation, transitions, or scroll-triggered reveals.

## Decisions

### Decision: paragraph-index targeting via post-processed `.Content`

`marginalia` entries reference paragraphs by 1-indexed position in the post body. The `single.html` override splits the rendered HTML on `</p>` boundaries, walks them in order, and after the Nth `</p>` injects a sibling `<aside class="marginalia ...">` carrying the note for paragraph N.

**Why:** zero markup burden on the author. Drop a note in front-matter; it shows up.

**Trade-off:** if Duncan later edits the post and inserts a paragraph before the marginalia target, the note's index shifts and it now hangs on the wrong paragraph. This is intentional — the v2 anchor enhancement (below) is the escape hatch. For v1, the author is on the hook to update indices when they edit older posts. We mitigate by recommending in the spec that marginalia be added to posts that are no longer being edited (which is the natural use case anyway).

**Edge cases:**
- Markdown that emits non-`<p>` block elements (lists, blockquotes, code blocks, headings) breaks the "paragraph N" mental model. We define "paragraph" precisely as "the Nth occurrence of `</p>` in the rendered HTML, counting from 1." Other block elements don't increment the index.
- Notes whose `paragraph` exceeds the actual paragraph count are dropped silently with a build-time `warnf` so the author notices in `hugo` output.

### Decision: `layouts/_default/single.html` as the override surface

We add a single override at `layouts/_default/single.html` rather than per-section overrides at `layouts/blog/single.html` and `layouts/journal/single.html`. Reason: the override only diverges from Congo's stock `single.html` in one place (the `.Content` block). A single template avoids drift between two near-identical files. The override gates marginalia rendering on `.Section` being `blog` or `journal` so it doesn't accidentally fire on About / Gallery / CV singles.

Implementation sketch (in `layouts/_default/single.html`):
```go-html-template
{{ define "wrap-content" }}
  {{ $marginalia := .Params.marginalia }}
  {{ if and $marginalia (or (eq .Section "blog") (eq .Section "journal")) }}
    {{ $html := .Content | emojify }}
    {{ $parts := split $html "</p>" }}
    <div class="marginalia-grid">
      <div class="marginalia-body">
        {{ range $i, $p := $parts }}
          {{ if lt $i (sub (len $parts) 1) }}
            {{ printf "%s</p>" $p | safeHTML }}
            {{ $idx := add $i 1 }}
            {{ range where $marginalia "paragraph" $idx }}
              <aside class="marginalia marginalia--mobile" data-date="{{ .date }}">
                <details>
                  <summary>{{ dateFormat "2006-01-02" .date }}</summary>
                  {{ .note | markdownify }}
                </details>
              </aside>
            {{ end }}
          {{ else }}
            {{ $p | safeHTML }}
          {{ end }}
        {{ end }}
      </div>
      <aside class="marginalia-rail" aria-label="Marginal notes">
        {{ range $marginalia }}
          <div class="marginalia marginalia--desktop" data-paragraph="{{ .paragraph }}" data-date="{{ .date }}">
            <span class="marginalia__date">{{ dateFormat "2006-01-02" .date }}</span>
            <div class="marginalia__note">{{ .note | markdownify }}</div>
          </div>
        {{ end }}
      </aside>
    </div>
  {{ else }}
    {{ .Content | emojify }}
  {{ end }}
{{ end }}
```
(Real implementation will copy the rest of Congo's `single.html` verbatim and replace only the `.Content` line — exact code is the implementation phase, not the design phase.)

### Decision: dual-render desktop + mobile, CSS-controlled visibility

Marginalia render twice in the DOM: once inline (mobile) and once in the right rail (desktop). CSS toggles `display: none` on the unused variant per breakpoint. Same content shows up in both places, ensuring accessibility tools and `prefers-reduced-motion` users can always read notes inline.

**Why dual-render instead of CSS-only repositioning:** absolutely positioning `<aside>` siblings of `<p>` to align with their target paragraph requires knowing each paragraph's pixel offset, which we can't compute statically. Splitting into two render passes keeps it CSS-only and Hugo-static-friendly.

**Trade-off:** notes appear twice in the page source. For SEO/screen-reader, we mark the desktop rail with `aria-hidden="true"` and let the mobile inline notes be the canonical readable copy. (The `aria-hidden` flips per breakpoint via JS? No — actually we can't, since we want zero JS. Instead: leave the mobile inline copy as the canonical version always; on desktop the rail is *visual decoration* that duplicates content already present in the DOM. Screen readers see both; we accept that minor duplication as the cost of zero-JS responsiveness, OR we use `aria-hidden="true"` on the rail at all breakpoints because the inline copy is always present in DOM order. Pick the latter.)

### Decision: vendored handwriting font

Font family: **Caveat** (Google Fonts, OFL-licensed). Two weights (400, 600). Vendored under `static/fonts/caveat/` as `.woff2`. `@font-face` declared in `assets/css/custom.css` with `font-display: swap`.

**Why Caveat:** legible at small sizes, well-paired with serif body text, and freely redistributable. Alternatives considered: Kalam (heavier visual weight, less legible at 14px), Architects Daughter (charming but only one weight, less flexible).

**Why vendored:** no third-party requests at runtime — keeps the site privacy-clean and offline-buildable. Adds ~40KB across two weights — acceptable.

### Decision: deterministic per-note rotation from date hash

Each note rotates a small amount (range: -2deg to +2deg) so they don't all sit perfectly square. Rotation derived from a stable hash of the date string in CSS (via `--marginalia-rot: <deg>;` set inline on the element from a Hugo-side `mod` of a date hash). Deterministic so the rotation is the same across builds — no visual "shimmer" on rebuild.

### Decision: scope to blog + journal only in v1

Gallery is photo-led, not text-led. About / CV / Homepage are layout-led, not paragraph-led. Project landing pages (`content/journal/<project>/_index.md`) are TBD — they're text-light index pages and v1 ignores them. The capability spec is written so a v2 can extend scope by relaxing one requirement clause.

### Decision: front-matter shape

```yaml
marginalia:
  - paragraph: 3
    date: 2027-11-03
    note: "I'd put this differently now — see [journal/2027-11/](/journal/2027-11/)."
  - paragraph: 7
    date: 2028-01-15
    note: "Aged better than expected."
```

Required: `paragraph` (int, ≥ 1), `date` (ISO date), `note` (markdown string). Optional in v1: none. Reserved for v2: `anchor` (string), `kind` (e.g., "correction" | "amplification" | "retraction" — would drive a color or icon variant).

## Risks / Trade-offs

- **Index drift on edits.** Already covered above. Mitigated by spec recommending marginalia on stable posts; v2 anchor escape hatch documented.
- **`</p>` split is fragile against custom HTML in markdown.** Posts that author raw HTML containing `<p>` tags inside other elements may produce unexpected paragraph counts. Acceptable for now — Duncan's posts are plain markdown. Spec calls this out so future-Duncan doesn't get surprised.
- **Duplicate DOM content (inline + rail).** Mitigated via `aria-hidden="true"` on the rail. SEO impact: trivial — same author's same words.
- **Visual noise on a post with many notes.** No hard cap in v1, but the design.md recommends keeping notes sparse (3-7 per post). If density becomes a problem we can add a `max-notes-visible` truncation in v2.
- **Print stylesheet.** Print should render notes inline as footnotes-after-paragraphs. We add `@media print` rules that show the mobile variant and hide the rail.
- **Theme upgrade risk.** Our `layouts/_default/single.html` override could drift from Congo's upstream `single.html` if Congo adds new article features (comments wrapper, new TOC modes). Mitigation: when bumping the Congo submodule, diff `themes/congo/layouts/single.html` against our override and re-port any divergence. Note this in the spec as a maintenance obligation.

## Open Questions

- **Project landing pages (`content/journal/<project>/_index.md`)**: explicitly out of scope for v1, but worth confirming. Decision in proposal: out of scope; revisit if a use case appears.
- **About page (`content/about/`)**: explicitly out of scope for v1. The About page is the most stable text on the site and a plausible candidate for v2 expansion (notes on how the author's self-description has shifted).
- **Should the rail show only notes whose target paragraph is currently in viewport?** That requires JS. v1 says no — show all notes statically. Revisit if pages grow long enough to make this annoying.
