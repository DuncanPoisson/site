---
id: sit-ls5r
status: open
deps: [sit-u7vk]
links: []
created: 2026-04-17T23:11:04Z
type: task
priority: 1
assignee: Duncan Haddock
external-ref: openspec:design-refinement
parent: sit-t34p
tags: [gallery, layout, content]
---
# Gallery capability: content scaffold + list/single layouts

Stand up the Gallery section end-to-end. Create content/gallery/_index.md with front matter (title='Gallery', a short description). Create one example collection as a page bundle at content/gallery/<slug>/_index.md with front matter (title, description, optional cover) and at least one placeholder image file inside the bundle directory so the collection page renders a non-empty photo grid (reuse static/images/example-thumb.png or any other suitable placeholder — copy, do not symlink). Create layouts/gallery/list.html: renders a responsive grid of collection cards (3 cols ≥1024px, 2 cols ≥640px, 1 col <640px) following the pattern in layouts/projects/list.html (rounded rectangle, cream background with burnt-orange border, hover lift), iterating .Pages and displaying each collection's title and description. Create layouts/gallery/single.html: renders the collection's title, description, and a responsive grid of image resources from .Resources.ByType 'image' (at least 2 cols on ≥640px, 1 col below). When a collection has zero image resources, show an empty-state message 'No photos yet.' — do not error or show a blank grid.

## Design

Gallery images are modeled as Hugo page-bundle resources (files co-located with the collection's _index.md) rather than per-photo content files — adding a photo is a matter of dropping a file into the collection directory, no frontmatter required. Mirror the card pattern from layouts/projects/list.html for visual continuity: background rgba(var(--color-neutral-50), 1), 2px border in secondary-500, border-radius ~0.5rem, padding 1.5rem, hover transform and box-shadow. In the single layout, iterate .Resources.ByType 'image' and render <img> elements with src=.RelPermalink and an alt derived from the file name (or from an optional Params.alt if we later support it). Empty-state check: {{ if .Resources.ByType "image" }}...grid...{{ else }}<p>No photos yet.</p>{{ end }}. Do NOT touch content/cv/ and do NOT remove anything under themes/congo/. The example collection should use a short human-readable slug (e.g., 'example', 'santa-fe-2026', etc.) — Duncan will rename or copy the pattern when he has real collections.

## Acceptance Criteria

content/gallery/_index.md exists with title='Gallery' and a description. At least one collection bundle exists at content/gallery/<slug>/_index.md with title/description front-matter and at least one image file committed inside the bundle directory. layouts/gallery/list.html renders /gallery/ as a card grid of collections — 3 cols at ≥1024px, 2 cols at ≥640px, 1 col below. layouts/gallery/single.html renders /gallery/<slug>/ with a responsive photo grid populated from image resources, and an empty-state message when a collection has zero image resources. hugo --minify exits 0 with zero ERROR lines; public/gallery/index.html exists and contains the collection card; public/gallery/<slug>/index.html exists and contains an <img> referencing the placeholder file. themes/congo/ unmodified; layouts/partials/functions/warnings.html unmodified. Clicking the Gallery nav item navigates to /gallery/; clicking the example collection card navigates to /gallery/<slug>/.

