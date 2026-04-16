---
id: sit-f5ks
status: closed
deps: [sit-k1d8]
links: []
created: 2026-04-15T23:48:41Z
type: task
priority: 1
assignee: Duncan Haddock
parent: sit-g16i
---
# Scaffold Blog + Research Journal with cross-links

Create content/blog/_index.md, content/blog/example-post.md (full frontmatter, >=2 headings for TOC); content/research/_index.md; content/research/example-project/_index.md; content/research/example-project/example-entry.md (tags, code block, image placeholder). Add mutual {{< ref >}} cross-links between example post and example entry. Document ref/relref convention in an authoring comment in each file.

## Acceptance Criteria

hugo --minify exits 0 with no ERROR (ref targets validated); reading time + TOC render on blog and research entries; a shared /tags/<term>/ page lists both a blog post and a research entry tagged with that term; cross-links resolve under /site/.

## Skills

- hugo-congo
- ui-ux
- backpressure

## Design

- Files: `content/blog/_index.md`, `content/blog/example-post.md`, `content/research/_index.md`, `content/research/example-project/_index.md`, `content/research/example-project/example-entry.md`.
- Research Journal organizes by project (decision #1): nested `_index.md` per project; entries are siblings.
- Cross-links MUST use `{{< ref "..." >}}` (decision #3). Both the blog post and the research entry reference each other — Hugo validates at build, so both files must exist in the same commit for the build to pass.
- Creation order within the ticket: create both `_index.md` files, the project `_index.md`, the research entry, then the blog post. Run `hugo --minify` only after all files exist.
- Frontmatter fields on example post/entry: `title`, `date`, `draft: false`, `tags`, `categories`, `description`. Share at least one tag between the post and the entry so `/tags/<term>/` surfaces both (acceptance).
- Example entry includes: a fenced code block, an image placeholder (use a relative `static/` asset or data URL), tags.
- Add a short HTML-comment authoring note in both files explaining `ref`/`relref` usage (decision #3, per task 5.2).
- Do NOT add custom layouts; rely on Congo defaults for section lists and single pages.


## Notes

**2026-04-16T19:25:53Z**

Created blog and research journal sections with cross-links. Blog has _index.md + example-post.md (3 headings for TOC, reading time). Research has _index.md, example-project/_index.md, and example-entry.md (code block, SVG image placeholder, 3 headings). Shared 'complexity' tag produces aggregated /tags/complexity/ page listing both. Mutual ref cross-links validated by hugo --minify (exit 0, no errors). HTML comment authoring notes use escaped shortcode syntax to avoid Hugo parsing.
