---
id: sit-8m3k
status: open
deps: []
links: []
created: 2026-04-16T21:10:00Z
type: task
priority: 1
assignee: Duncan Haddock
external-ref: openspec:design-overhaul
parent: sit-0mai
tags: [layout, content]
---
# Rename Research to Journal and create box-grid list layout

git mv content/research/ content/journal/. Update all ref/relref shortcodes across content files from /research/... to /journal/.... Create layouts/journal/list.html with CSS Grid box layout: 3 columns desktop (>=1024px), 2 tablet (640-1023px), 1 mobile (<640px). Boxes: warm cream background, burnt orange border, green title text. Each box shows project title and description from _index.md frontmatter. Clicking navigates to project landing page.

## Design

Hugo resolves section names from content directory path. Layout override via layouts/journal/list.html. Use CSS Grid with media queries. Style block inline in layout template. Extend Congo base template with define main.

## Acceptance Criteria

content/journal/ exists with preserved project structure. All cross-links updated. layouts/journal/list.html renders responsive box-grid. hugo --minify passes with no broken refs.

