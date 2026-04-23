---
id: sit-ojxx
status: closed
deps: []
links: []
created: 2026-04-23T17:09:11Z
type: task
priority: 1
assignee: Duncan Haddock
external-ref: openspec:add-marginalia
parent: sit-rsns
tags: [marginalia, css, font]
---
# Vendor Caveat font + base CSS for marginalia (grid, rail, mobile inline, print)

Vendor Caveat (400 + 600 weights, woff2) under static/fonts/caveat/ with the OFL license file. Add an @font-face block for Caveat in assets/css/custom.css with font-display: swap. Add base CSS rules for .marginalia-grid, .marginalia-body, .marginalia-rail, .marginalia, .marginalia--desktop, .marginalia--mobile, .marginalia__date, .marginalia__note covering: desktop two-column grid (article + rail) at the lg: breakpoint; mobile inline <details> styling; sticky-note background derived from the secondary scale; deterministic per-note rotation via a --marginalia-rot custom property; print stylesheet hiding the rail and showing inline notes expanded.

## Design

Caveat ships under static/fonts/caveat/ as Caveat-Regular.woff2 + Caveat-SemiBold.woff2 + OFL.txt. The @font-face uses local() fallback then url('/fonts/caveat/Caveat-Regular.woff2') format('woff2'). The desktop grid uses CSS grid with two columns (article + ~16rem rail) at min-width: 1024px; below that the rail collapses (display: none). Mobile inline notes use the <details>/<summary> pattern with the date as summary text. Sticky-note background uses rgba(var(--color-secondary-100), 0.6) or similar from the existing secondary scale — do NOT introduce new color tokens. Rotation is set per element via inline style: --marginalia-rot: <deg>; and applied via transform: rotate(var(--marginalia-rot)). Print rules: @media print { .marginalia-rail { display: none } .marginalia--mobile details { display: block } .marginalia--mobile details[open] {} .marginalia--mobile details summary { ... } }.

## Acceptance Criteria

static/fonts/caveat/ contains Caveat-Regular.woff2, Caveat-SemiBold.woff2, and OFL.txt. assets/css/custom.css contains an @font-face for Caveat (font-display: swap, family 'Caveat'). assets/css/custom.css contains all marginalia rules listed above. Loading any page makes zero requests to fonts.googleapis.com or fonts.gstatic.com. hugo --minify exits 0. CSS rules don't leak onto pages without marginalia (rules are scoped to .marginalia-grid descendants).

