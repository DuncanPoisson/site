---
id: sit-tezt
status: closed
deps: [sit-qks1, sit-gvo4, sit-gp18]
links: []
created: 2026-04-16T23:40:01Z
type: task
priority: 2
assignee: Duncan Haddock
external-ref: openspec:custom-homepage
parent: sit-3tct
tags: [build, qa]
---
# Validate custom-homepage build and visual check

Run hugo --minify and confirm exit 0 with zero 'ERROR' lines in combined output. Run hugo server and visually verify the homepage in light mode, dark mode, desktop width, and mobile (~375px) width: hero stacks correctly, three cards collapse to a single column on mobile, dividers render burnt orange, name heading is green, quote is centered italic. Click each of the four cards to confirm they navigate to /blog/, /journal/, /projects/, and https://linktr.ee/. Confirm themes/congo/ is clean per git status and layouts/partials/functions/warnings.html is byte-identical to main.

## Design

Use curl or a browser for visual check. For a mobile-width check, resize the window or use browser devtools responsive mode. No automated Playwright harness in this project — manual visual verification is acceptable. If anything fails, reopen the upstream ticket rather than patching here.

## Acceptance Criteria

hugo --minify exits 0 with no ERROR lines. All four card click targets navigate correctly. Homepage renders correctly at desktop and <=768px widths in both light and dark modes. themes/congo/ unchanged. warnings.html unchanged.


## Notes

**2026-04-17T18:25:14Z**

Validation:
- hugo --minify: exit 0, zero ERROR lines (36 pages, 9 static files)
- GET /site/ returns 200; served HTML contains Duncan Xavier Haddock header anchor (rel=me), home-hero section, 25 home-card tokens (cards + subtitle spans), all four copy blocks (Howdy, I find myself, By training, Angela Davis), /linktr.ee/ link, and contains zero 'Duncan Poisson' references
- GET /site/images/duncan.jpg returns 200
- GET /site/blog/, /site/journal/, /site/projects/ all return 200
- themes/congo/ unchanged vs main (git diff --stat empty)
- layouts/partials/functions/warnings.html unchanged vs main
