---
id: sit-h2q2
status: closed
deps: []
links: []
created: 2026-04-17T23:09:58Z
type: task
priority: 1
assignee: Duncan Haddock
external-ref: openspec:design-refinement
parent: sit-t34p
tags: [design, color-scheme, css]
---
# Regenerate duncan.css palette (primary #082620, neutral #ead4bb) + header band in custom.css

Rewrite assets/css/schemes/duncan.css so --color-primary-* is re-derived around forest green #082620 at the 500 level and --color-neutral-* is re-derived around #ead4bb (mapped to neutral-50) with the deep end sliding toward body-text color #0e1416. Keep --color-secondary-* (burnt orange #C74D20) unchanged. Regenerate the dark-mode block so primary 500 in dark mode is a brighter forest green that meets WCAG AA contrast against the dark-neutral background. In assets/css/custom.css: update the header site-title rule's color value from #429347 to #082620 (keep the rel='me' selector), preserve the existing burnt-orange <hr> rule, and add a new rule that sets the header/nav band background to #d1baa3 — validate the Congo header selector against rendered public/ output and pick the narrowest binding.

## Design

Congo reads --color-neutral-*, --color-primary-*, --color-secondary-* as RGB triplets; re-deriving the full 50→950 scale means most surfaces (links, tags, buttons, footer) update automatically without per-component overrides. For the header band, inspect Congo's rendered header element (likely header.sticky, header nav, or the wrapping element carrying bg-neutral classes) and pick the narrowest selector that binds both light and dark variants. If the CSS selector proves fragile against Congo upgrades, fall back to overriding layouts/partials/header.html rather than broadening the selector. Do NOT modify anything under themes/congo/.

## Acceptance Criteria

duncan.css primary 500 computes to rgb(8, 38, 32) in light mode; neutral 50 computes to rgb(234, 212, 187). Secondary palette byte-for-byte unchanged. Dark-mode primary 500 is a brighter forest green that clears WCAG AA contrast (>=4.5:1) against the dark-neutral background. custom.css has exactly one rule setting the header band background to rgb(209, 186, 163) and one updated site-title rule using #082620. themes/congo/ unmodified. hugo --minify exits 0 with zero ERROR lines. Rendered site in the browser shows a visible contrast between the header band and the page body.


## Notes

**2026-04-17T23:18:49Z**

Regenerated duncan.css around primary #082620 and neutral #ead4bb mapped to neutral-50. Primary-500 light = rgb(8,38,32); dark-mode primary-500 = rgb(88,165,120) for AA contrast against dark neutrals. Neutral-900 light = rgb(14,20,22) = #0e1416 for body text. Secondary (burnt orange) unchanged. custom.css: site-title color now #082620; added body > header rule using box-shadow + clip-path to extend the #d1baa3 band edge-to-edge without widening body. Dark-mode band = #2a251f. hugo --minify exit 0, no ERROR lines.
