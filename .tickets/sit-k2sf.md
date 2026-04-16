---
id: sit-k2sf
status: closed
deps: [sit-q0fm, sit-ft8b]
links: []
created: 2026-04-16T21:10:23Z
type: task
priority: 2
assignee: Duncan Haddock
external-ref: openspec:design-overhaul
parent: sit-0mai
tags: [css, design]
---
# Add burnt orange visual dividers via custom.css

Create assets/css/custom.css with hr styling: solid burnt orange (#C74D20), 2-3px thick, appropriate margins. Add --- dividers to content/_index.md between hero and about sections.

## Design

Congo auto-loads assets/css/custom.css if present. Style hr globally. Use RGB of C74D20 = 199, 77, 32.

## Acceptance Criteria

assets/css/custom.css exists. All hr elements render as burnt orange lines. Homepage has visible dividers between sections.

