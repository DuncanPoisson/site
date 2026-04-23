---
id: sit-xmcn
status: closed
deps: [sit-7zr3]
links: []
created: 2026-04-23T17:09:57Z
type: task
priority: 2
assignee: Duncan Haddock
external-ref: openspec:add-marginalia
parent: sit-rsns
tags: [marginalia, validation, qa]
---
# Validate marginalia: hugo build, manual walk-through, openspec validate

Run hugo --minify and verify exit 0 with zero ERROR lines. Spin up hugo server and walk through both example pages at desktop (≥ 1024px) and mobile (< 1024px) widths in light + dark mode: confirm rail alignment vs target paragraphs, mobile <details> collapse + expand, deterministic rotation, no font flash, and that pages without marginalia render unchanged. Print-preview both example pages: confirm rail is hidden and notes appear inline expanded. Inspect built page network panel: confirm zero requests to fonts.googleapis.com or fonts.gstatic.com. Run npx openspec validate add-marginalia --strict and confirm it passes. Append a learning to LESSONS.md if anything non-obvious surfaced (paragraph-counting gotchas, Hugo template quirks, CSS ordering issues, font-loading nuances).

## Acceptance Criteria

hugo --minify exits 0 with zero ERROR lines. Manual walk-through complete with no visual regressions on pages without marginalia. Print preview shows inline expanded notes with no rail. Network panel confirms zero third-party font requests. npx openspec validate add-marginalia --strict passes. LESSONS.md updated if anything non-obvious was learned.

