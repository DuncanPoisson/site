---
id: sit-u20y
status: open
deps: [sit-h2q2, sit-jdig, sit-u7vk, sit-ls5r]
links: []
created: 2026-04-17T23:11:20Z
type: task
priority: 1
assignee: Duncan Haddock
external-ref: openspec:design-refinement
parent: sit-t34p
tags: [qa, validation]
---
# Final visual QA: desktop + mobile, light + dark; verify untouched overrides

Once palette, homepage, nav, and gallery tickets are closed, run the whole-site visual verification. Start 'hugo server' locally and walk through: (a) homepage section 1 hero unchanged; (b) section 2 shows the matrix on desktop with text-left+photo-right in row 1 and photo-left+text-right in row 2, and stacks correctly on a ≤768px viewport with the specified order per row; (c) all four bubbles (Blog, Journal, Projects, More of Me) show no list markers, centered titles, left-aligned description copy, background #082620 and text #EAF9E1; (d) section-3 More-of-Me bubble is horizontally centered at 50-60% container width with centered text and correct Linktree URL; (e) primary nav shows About / Blog / Journal / Gallery / Projects in that order; (f) /gallery/ renders the example collection card grid; (g) clicking the collection card opens the per-collection photo grid; (h) /cv/ still resolves when typed directly; (i) the header/nav band color #d1baa3 is visibly distinct from the page background #ead4bb; (j) dark-mode toggle produces a coherent warm-dark palette with legible primary/secondary text. Run hugo --minify one last time and verify zero ERROR lines. Git-diff themes/congo/ and layouts/partials/functions/warnings.html against main to confirm zero changes.

## Design

This is a gated manual QA ticket. It does not write code — it only runs hugo server, opens a browser, and eyeballs the scenarios in the acceptance criteria. If any scenario fails, reopen the corresponding upstream ticket (sit-h2q2 for palette issues, sit-jdig for homepage issues, sit-u7vk for nav issues, sit-ls5r for gallery issues) rather than patching here. Add any non-obvious learnings to LESSONS.md before closing.

## Acceptance Criteria

All ten QA scenarios (a–j) in the description pass. hugo --minify exits 0 with zero ERROR lines. 'git diff main -- themes/congo/' and 'git diff main -- layouts/partials/functions/warnings.html' both produce zero output. LESSONS.md has been updated if any non-obvious discovery was made during implementation.

