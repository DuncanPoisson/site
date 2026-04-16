---
id: sit-ft8b
status: closed
deps: []
links: []
created: 2026-04-16T21:09:36Z
type: task
priority: 1
assignee: Duncan Haddock
external-ref: openspec:design-overhaul
parent: sit-0mai
tags: [design, css]
---
# Create custom color scheme (duncan.css) with light and dark mode palettes

Create assets/css/schemes/duncan.css. Light mode: warm cream neutrals from #fff6ed, forest green primary scale (50-950) centered on #429347, burnt orange secondary scale (50-950) centered on #C74D20. Dark mode: warm-dark background (not pure black), keep green primary and orange secondary. Set colorScheme = duncan in params.toml.

## Design

Follow Congo scheme format (see themes/congo/assets/css/schemes/congo.css). Use RGB triplets without # prefix. Generate perceptually balanced scales around the 500 anchor values.

## Acceptance Criteria

duncan.css exists with complete --color-neutral-*, --color-primary-*, --color-secondary-* variables in RGB triplet format for both :root and .dark selectors. params.toml has colorScheme = duncan. Site renders with no purple elements.

