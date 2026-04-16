---
id: sit-xbij
status: open
deps: []
links: []
created: 2026-04-16T21:10:07Z
type: task
priority: 2
assignee: Duncan Haddock
external-ref: openspec:design-overhaul
parent: sit-0mai
tags: [layout]
---
# Create blog image+text list layout

Create layouts/blog/list.html with horizontal layout: optional thumbnail image on left (~180px wide), title + date + description on right. If no thumbnail in frontmatter, show text only. Mobile (<640px): stack vertically, image on top. Add thumbnail field to content/blog/example-post.md frontmatter with a sample image.

## Design

Use flexbox rows. Check .Params.thumbnail for image path. No placeholder image for missing thumbnails. Style block inline in layout template.

## Acceptance Criteria

layouts/blog/list.html renders horizontal rows. Posts with thumbnail show image+text. Posts without thumbnail show text only. Mobile stacks vertically. Example post has thumbnail field.

