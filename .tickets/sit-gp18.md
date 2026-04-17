---
id: sit-gp18
status: closed
deps: []
links: []
created: 2026-04-16T23:39:29Z
type: task
priority: 2
assignee: Duncan Haddock
external-ref: openspec:custom-homepage
parent: sit-3tct
tags: [assets]
---
# Commit placeholder photo at static/images/duncan.jpg

Create a small placeholder JPG at static/images/duncan.jpg so the new homepage <img> has a valid target before a real photo is supplied. Note in the commit message (or an adjacent TODO comment in the homepage layout) that the user will replace this file with a real ~800x800 JPG.

## Design

Any valid, small JPG works — e.g., a neutral-colored square or the theme's default avatar sized down. The key constraint: file must exist and be a parseable image so Hugo's static pipeline doesn't 404. Do not reference the file via Hugo's image processing pipeline (no resources.GetMatch / .Resize) since it's intended to be swapped raw.

## Acceptance Criteria

static/images/duncan.jpg exists on disk and is a valid JPG. hugo --minify succeeds. After the layout ticket ships, requesting /images/duncan.jpg from hugo server returns 200.


## Notes

**2026-04-17T17:52:46Z**

Created static/images/duncan.jpg (800x534 baseline JPEG, 60KB) from the existing example-thumb.png via sips. User should replace with a real ~800x800 portrait JPG.
