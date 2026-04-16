---
id: sit-q0fm
status: closed
deps: [sit-8m3k]
links: []
created: 2026-04-16T21:09:50Z
type: task
priority: 1
assignee: Duncan Haddock
external-ref: openspec:design-overhaul
parent: sit-0mai
tags: [navigation, content]
---
# Restructure navigation and merge homepage with about page

Update menus.toml: About (-> /, weight 10), Blog (-> /blog/, weight 20), Journal (-> /journal/, weight 30), CV (-> /cv/, weight 40), Projects (-> /projects/, weight 50). Remove separate Home item. Merge content/about/_index.md into content/_index.md with hero section (name + tagline) and divider. Delete content/about/ directory.

## Design

Congo page homepage layout already renders _index.md as content page. Just update menus.toml and merge about text into _index.md.

## Acceptance Criteria

Nav shows 5 items in order: About, Blog, Journal, CV, Projects. About links to /. No separate /about/ URL. Homepage displays name, tagline, and about content.

