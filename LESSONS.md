# Lessons Learned

Cumulative learnings from development iterations. Each entry references a ticket ID.

---

### sit-o0z6 — `.Content` keeps `</p>` boundaries even under `hugo --minify`

Hugo's HTML minifier strips closing `</p>` (and other optional closers) from the FINAL output, but `.Content` exposed inside templates is the rendered-markdown HTML BEFORE minification. So template logic that splits `.Content` on `</p>` works under both `hugo` and `hugo --minify` builds — the minifier runs on the bytes after templates emit them, not on `.Content` itself. Verified by building both ways and confirming the marginalia paragraph counter resolves identically.

Also: when fetching webfonts from Google Fonts CSS API for vendoring, the user-agent matters. Default `curl` UA gets `.ttf` URLs; modern Chrome/Firefox UAs get `.woff2`. Set `User-Agent: Mozilla/5.0 ... Chrome/...` to receive the woff2 responses. Caveat (and many recent additions) is served as a variable font where weight-400 and weight-600 resolve to the same woff2 file with the weight axis controlling rendering.

### sit-h2q2 — full-width header band on a centered body

Congo's `<body>` is `m-auto flex max-w-7xl`, so the `<header>` is clipped to the centered content width. Setting `background-color` alone produces a content-width band, not a site-wide bar. To extend the band edge-to-edge without widening the body, use `box-shadow: 0 0 0 100vmax <color>` combined with `clip-path: inset(0 -100vmax)` on the header — the shadow paints horizontally past the viewport and the clip-path removes vertical overflow. No body `overflow-x` change needed; no horizontal scrollbar appears.

### design-refinement archive — OpenSpec 1.3 main-spec format + MODIFIED header matching

OpenSpec 1.3's archiver expects main specs under `openspec/specs/<cap>/spec.md` in the format `# <cap> Specification\n\n## Purpose\n...\n## Requirements\n### Requirement: ...`. Specs written by older OpenSpec versions (starting directly with `## ADDED Requirements`) parse fine for validation and for ADDED-only archives, but MODIFIED deltas fail with `MODIFIED failed for header "### Requirement: X" - not found` because the archiver can't locate the requirement section. Fix: prepend `# <cap> Specification\n\n## Purpose\nTBD\n\n## Requirements\n` and rename `## ADDED Requirements` to `## Requirements` in the main spec before archiving. Additionally, MODIFIED delta headers must match the original main-spec header verbatim — if the requirement is being renamed, keep the old title in the delta (body content describes the new behavior) or use REMOVED + ADDED instead.

### sit-ls5r — single list.html for top-level + nested sections

Hugo's template lookup for `Kind: section` can't distinguish a top-level section (`/gallery/`) from a nested one (`/gallery/example/`) by file name — both resolve to `layouts/<section>/list.html` (or `section.html`). The cleanest way to render collection cards at the index and photo grids inside each collection, when collections are `_index.md` bundles, is a single `list.html` that branches on `.Pages`: non-empty → iterate child pages as cards; empty → iterate `.Resources.ByType "image"` as a photo grid. Save a reference to the ranged page (`{{ $page := . }}`) before entering `{{ with .Params.cover }}` because `with` rebinds `.` to the param value and you lose access to the page's resources otherwise.
