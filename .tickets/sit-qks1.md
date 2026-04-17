---
id: sit-qks1
status: closed
deps: []
links: []
created: 2026-04-16T23:39:21Z
type: task
priority: 1
assignee: Duncan Haddock
external-ref: openspec:custom-homepage
parent: sit-3tct
tags: [config, css, rename]
---
# Rename site to Duncan Xavier Haddock and recolor header title green

Update config/_default/hugo.toml title and config/_default/params.toml [author].name to 'Duncan Xavier Haddock'. Sweep remaining 'Duncan Poisson' references in tracked files outside themes/, public/, .git/, openspec/changes/archive/ (notably content/_index.md front matter once it is rewritten by the layout ticket; IMPLEMENTATION_PLAN.md was already updated in interview mode but double-check). Append a CSS rule in assets/css/custom.css that recolors the header/nav site title to primary green #429347; verify the selector against the built HTML.

## Design

Prefer CSS-only override of the header title color to avoid forking themes/congo/layouts/partials/header/basic.html. Likely selectors: .site-title, header a[href='/'], or similar — confirm against public/index.html after hugo --minify. Use var(--color-primary-500) if available from duncan.css, else the literal #429347 with a comment pointing to the color-scheme spec. Leave dividers rule untouched.

## Acceptance Criteria

grep for 'Duncan Poisson' across tracked files (excluding themes/, public/, .git/, openspec/changes/archive/) returns zero matches. hugo.toml title = 'Duncan Xavier Haddock'. params.toml [author].name = 'Duncan Xavier Haddock'. In the built site, the header site-title element's computed color resolves to #429347. No file under themes/congo/ modified. layouts/partials/functions/warnings.html untouched. hugo --minify exits 0 with no ERROR lines.


## Notes

**2026-04-17T17:51:38Z**

Updated hugo.toml title, params.toml author name, content/_index.md front matter, and IMPLEMENTATION_PLAN.md. Appended CSS rule to assets/css/custom.css targeting header nav a[rel='me']. Verified built public/index.html renders Duncan Xavier Haddock and the header anchor carries rel=me. Remaining 'Duncan Poisson' references live only in change artifacts (openspec/changes/custom-homepage/) and tickets describing the rename — those will be excluded after archive.
