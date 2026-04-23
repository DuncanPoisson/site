---
id: sit-rsns
status: closed
deps: []
links: []
created: 2026-04-23T17:08:54Z
type: epic
priority: 1
assignee: Duncan Haddock
external-ref: openspec:add-marginalia
tags: [marginalia, layout, css, font]
---
# Marginalia: dated paragraph-pinned author notes for blog + journal

Each blog post and journal entry grows a margin of dated, handwritten-style notes that future-Duncan pins to specific paragraphs over time. Authored entirely in front-matter (paragraph index + ISO date + markdown note). Desktop renders notes in a right-side gutter; mobile collapses them inline as <details>. Self-hosted Caveat webfont, deterministic per-note rotation, scope gated to blog + journal sections. Theme files under themes/congo/ unmodified.

## Acceptance Criteria

All requirements in openspec/changes/add-marginalia/specs/ satisfied: marginalia (new), blog-layout (added), journal-layout (added). hugo --minify exits 0 with zero ERROR lines. npx openspec validate add-marginalia --strict passes. Manual visual QA: desktop ≥ 1024px and mobile < 1024px, light + dark, print preview. Zero third-party font requests at runtime. themes/congo/ unmodified.

