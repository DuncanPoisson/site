---
id: sit-t34p
status: open
deps: []
links: []
created: 2026-04-17T23:09:39Z
type: epic
priority: 1
assignee: Duncan Haddock
external-ref: openspec:design-refinement
tags: [design, layout, content, gallery]
---
# Design refinement: palette, matrix homepage, gallery, CV→Gallery swap

Refine site visuals: darker forest-green primary (#082620), warmer tan background (#ead4bb) with a distinct header band (#d1baa3), body text #0e1416, light-mint (#EAF9E1) text inside green bubbles. Rewrite homepage section 2 as a two-row photo/text matrix using static/images/photo1.png and photo2.png with verbatim authored copy. Center and widen the More-of-Me bubble and link it to https://linktr.ee/duncanpoisson. Remove the CV nav entry (CV page stays reachable) and introduce a Gallery section at /gallery/ with a collection-card grid and per-collection photo grid. Fix bubble list markers and text alignment.

## Acceptance Criteria

All delta specs in openspec/changes/design-refinement/ satisfied: color-scheme, homepage-layout, site-identity, gallery. hugo --minify exits 0 with zero ERROR lines. Manual visual QA: desktop + mobile, light + dark. themes/congo/ unmodified; layouts/partials/functions/warnings.html unmodified.

