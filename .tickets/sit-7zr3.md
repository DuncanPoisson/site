---
id: sit-7zr3
status: closed
deps: [sit-ojxx, sit-o0z6]
links: []
created: 2026-04-23T17:09:45Z
type: task
priority: 2
assignee: Duncan Haddock
external-ref: openspec:add-marginalia
parent: sit-rsns
tags: [marginalia, content]
---
# Add example marginalia to example blog post + example journal entry

Add 2-3 example marginalia entries to the existing example blog post (under content/blog/) covering different paragraphs and dates. Add 2-3 example marginalia entries to the existing example journal entry (under content/journal/<project>/). Notes should demonstrate the feel of the feature — short, dated, slightly conversational with their past selves; one with a markdown link in the note body so the markdownify path is exercised.

## Design

Locate the example blog post and example journal entry currently in the repo. Add a marginalia: list to each post's front-matter following the shape: marginalia: [ { paragraph: <int>, date: <ISO date>, note: <markdown string> } ]. Pick paragraph indices that actually exist in each post's body. Date stamps should look natural — pick a couple recent-looking dates and one a few months out from the post's publish date.

## Acceptance Criteria

The example blog post and example journal entry both have a marginalia: front-matter list with at least 2 entries each. Building the site shows marginalia visibly on both pages at desktop and mobile widths. At least one note's body contains a markdown link rendered as an <a> in the output.

