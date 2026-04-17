---
id: sit-u7vk
status: open
deps: []
links: []
created: 2026-04-17T23:10:43Z
type: task
priority: 1
assignee: Duncan Haddock
external-ref: openspec:design-refinement
parent: sit-t34p
tags: [nav, config]
---
# Replace CV nav item with Gallery in menus.toml

Edit config/_default/menus.toml: remove the [[main]] entry for CV, insert a new entry for Gallery (name='Gallery', pageRef='gallery', weight=40), and bump the Projects entry weight from 40 to 50. Leave About/Blog/Journal weights unchanged (10/20/30). Do NOT remove or rename content/cv/ — the /cv/ URL must continue to resolve via a direct link even though it is unreferenced from the nav.

## Design

Single-file change in config/_default/menus.toml. Weights become: About=10, Blog=20, Journal=30, Gallery=40, Projects=50. CV content at content/cv/_index.md stays untouched.

## Acceptance Criteria

config/_default/menus.toml has five [[main]] entries in order About/Blog/Journal/Gallery/Projects with weights 10/20/30/40/50. No CV entry in the nav. content/cv/ is unchanged on disk. After running hugo --minify, public/cv/index.html still builds. hugo --minify exits 0 with zero ERROR lines.

