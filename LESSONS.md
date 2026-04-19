# Lessons Learned

Cumulative learnings from development iterations. Each entry references a ticket ID.

---

### sit-h2q2 — full-width header band on a centered body

Congo's `<body>` is `m-auto flex max-w-7xl`, so the `<header>` is clipped to the centered content width. Setting `background-color` alone produces a content-width band, not a site-wide bar. To extend the band edge-to-edge without widening the body, use `box-shadow: 0 0 0 100vmax <color>` combined with `clip-path: inset(0 -100vmax)` on the header — the shadow paints horizontally past the viewport and the clip-path removes vertical overflow. No body `overflow-x` change needed; no horizontal scrollbar appears.

### design-refinement archive — OpenSpec 1.3 main-spec format + MODIFIED header matching

OpenSpec 1.3's archiver expects main specs under `openspec/specs/<cap>/spec.md` in the format `# <cap> Specification\n\n## Purpose\n...\n## Requirements\n### Requirement: ...`. Specs written by older OpenSpec versions (starting directly with `## ADDED Requirements`) parse fine for validation and for ADDED-only archives, but MODIFIED deltas fail with `MODIFIED failed for header "### Requirement: X" - not found` because the archiver can't locate the requirement section. Fix: prepend `# <cap> Specification\n\n## Purpose\nTBD\n\n## Requirements\n` and rename `## ADDED Requirements` to `## Requirements` in the main spec before archiving. Additionally, MODIFIED delta headers must match the original main-spec header verbatim — if the requirement is being renamed, keep the old title in the delta (body content describes the new behavior) or use REMOVED + ADDED instead.

### sit-ls5r — single list.html for top-level + nested sections

Hugo's template lookup for `Kind: section` can't distinguish a top-level section (`/gallery/`) from a nested one (`/gallery/example/`) by file name — both resolve to `layouts/<section>/list.html` (or `section.html`). The cleanest way to render collection cards at the index and photo grids inside each collection, when collections are `_index.md` bundles, is a single `list.html` that branches on `.Pages`: non-empty → iterate child pages as cards; empty → iterate `.Resources.ByType "image"` as a photo grid. Save a reference to the ranged page (`{{ $page := . }}`) before entering `{{ with .Params.cover }}` because `with` rebinds `.` to the param value and you lose access to the page's resources otherwise.
