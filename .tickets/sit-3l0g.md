---
id: sit-3l0g
status: closed
deps: [sit-k1d8]
links: []
created: 2026-04-15T23:48:40Z
type: task
priority: 2
assignee: Duncan Haddock
parent: sit-g16i
---
# Scaffold top-level content pages (home, about, CV, questions)

Create content/_index.md (Congo page layout, placeholder statement, link to /about/); content/about/_index.md (single page placeholder); content/cv/_index.md (Education/Experience/Skills/Relevant Coursework sections); content/questions/_index.md (question-tracker table: question, date logged, source post link, status).

## Acceptance Criteria

hugo --minify exits 0; routes /, /about/, /cv/, /questions/ resolve under /site/; CV page shows all four sections; Questions page renders example table.

## Skills

- hugo-congo
- ui-ux
- backpressure

## Design

- Files: `content/_index.md`, `content/about/_index.md`, `content/cv/_index.md`, `content/questions/_index.md`.
- Homepage uses Congo `page` layout (decision #6) — brief placeholder + link to `/about/`.
- Mark placeholder content clearly (e.g., HTML comment `<!-- placeholder: replace before publish -->` or a visible "Draft placeholder" admonition).
- Questions page uses a markdown table with columns: Question | Logged | Source | Status. One example row.
- All internal links use `{{< ref >}}` / `{{< relref >}}` (decision #3), not raw paths.
- No draft frontmatter on `_index.md` files (section indexes must render).


## Notes

**2026-04-16T19:32:06Z**

Created content/_index.md (homepage with page layout and link to about), content/about/_index.md, content/cv/_index.md (Education/Experience/Skills/Relevant Coursework sections), content/questions/_index.md (question tracker table with example row). All pages use [placeholder] markers. hugo --minify passes clean.
