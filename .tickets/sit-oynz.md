---
id: sit-oynz
status: open
deps: []
links: []
created: 2026-04-16T21:10:15Z
type: task
priority: 2
assignee: Duncan Haddock
external-ref: openspec:design-overhaul
parent: sit-0mai
tags: [layout, content]
---
# Create Projects portfolio section with card-grid layout

Create content/projects/_index.md (landing page) and content/projects/example-project.md (placeholder with status, tags, links fields). Create layouts/projects/list.html with card-grid layout matching journal grid style: 3/2/1 responsive columns. Cards show title, description, and status badge (completed/ongoing).

## Design

Same CSS Grid pattern as journal layout. Status badge via .Params.status. Project frontmatter: title, description, status (completed/ongoing), tags, links (list of labeled URLs).

## Acceptance Criteria

content/projects/ exists with _index.md and example project. layouts/projects/list.html renders responsive card-grid with status badges. /projects/ accessible via nav.

