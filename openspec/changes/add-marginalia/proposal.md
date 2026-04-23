## Why

Personal sites are usually write-once: a post lands and then sits frozen, even as the author's relationship to it changes. We want each long-form post to grow visible scar-tissue of revisits — small dated notes pinned to specific paragraphs by future-Duncan ("disagree now," "this aged well," "see also journal/2027-11-03"). It's an old scholarly gesture (marginalia) turned inward, and it makes the site read less like a bulletin board and more like a working notebook.

## What Changes

- New `marginalia` capability: a Hugo-native system for attaching small, dated, paragraph-pinned notes to a post over time, authored entirely in front-matter.
- Author surface: a `marginalia:` list in the front-matter of any blog post or journal entry. Each entry has `paragraph` (1-indexed paragraph index in the post body), `date` (ISO date), `note` (markdown), optional `anchor` (to override paragraph indexing — see "Future" in design.md, not v1).
- Render surface (desktop ≥ 1024px): article body stays in its `max-w-prose` column; marginalia float in a right gutter, each as a small handwritten-style sticky note next to the targeted paragraph, with a slight deterministic rotation and a date stamp.
- Render surface (mobile / tablet < 1024px): each note collapses to an inline `<details>` block immediately after its paragraph, summary = date, body = note.
- Style: a self-hosted handwriting webfont (Caveat or similar from Google Fonts, vendored under `static/fonts/`); note background derived from the existing secondary scale; date stamped in small caps. No new color tokens added.
- Scope (v1): blog posts (`content/blog/*.md`) and journal entries (`content/journal/<project>/*.md`). Project landing pages, About, CV, Gallery, and homepage are out of scope.
- Local override of `single.html`: a new `layouts/_default/single.html` (or per-section overrides under `layouts/blog/single.html` and `layouts/journal/single.html`) that wraps `.Content` rendering to inject marginalia next to paragraphs. Theme files under `themes/congo/` are NOT modified.
- Example content: at least one blog post and one journal entry SHALL ship with example marginalia in front-matter so the feature is visible at first build.
- Spec updates: new `marginalia` spec; MODIFIED requirements added to `blog-layout` and `journal-layout` capturing the new render expectations on those sections.

## Capabilities

### New Capabilities

- `marginalia`: dated paragraph-pinned author notes for long-form posts — front-matter authoring shape, render rules (desktop gutter / mobile inline), deterministic styling (rotation, sticky-note color), and scope (blog + journal entries).

### Modified Capabilities

- `blog-layout`: add a requirement that a post-rendering surface for blog posts SHALL render marginalia from the post's front-matter according to the `marginalia` capability.
- `journal-layout`: add the same render-surface requirement for journal entries.

## Impact

- **Layouts**: new `layouts/_default/single.html` (or section overrides) wrapping `.Content` with paragraph-walking + marginalia injection.
- **CSS**: new rules in `assets/css/custom.css` for `.marginalia`, `.marginalia--desktop`, `.marginalia--mobile`, `.marginalia__date`, plus a CSS grid wrapper around the article body that opens up a right gutter at `lg:` breakpoint.
- **Fonts**: one vendored handwriting webfont under `static/fonts/` plus an `@font-face` declaration in `custom.css`. No external font requests at runtime.
- **Content**: example marginalia added to the existing example blog post and example journal entry.
- **Theme**: zero changes under `themes/congo/`.
- **Build**: `hugo --minify` SHALL exit 0 with no `ERROR` lines on a build that includes example marginalia.
- **Out of scope (and explicitly so)**: a JS-driven authoring UI, a CMS, anchor-based targeting (a future enhancement), marginalia on landing/index/section pages, marginalia on Gallery/CV/About, and any persistence layer beyond Hugo front-matter.
