---
id: sit-o0z6
status: open
deps: []
links: []
created: 2026-04-23T17:09:34Z
type: task
priority: 1
assignee: Duncan Haddock
external-ref: openspec:add-marginalia
parent: sit-rsns
tags: [marginalia, layout, hugo]
---
# Add layouts/_default/single.html with paragraph-walking marginalia injection

Create layouts/_default/single.html as a verbatim copy of themes/congo/layouts/single.html, then replace the {{ .Content | emojify }} block (around line 45) with a wrapper that injects marginalia. Gate marginalia rendering on (or (eq .Section "blog") (eq .Section "journal")) AND a non-empty .Params.marginalia. When gated in: split the rendered .Content on </p> boundaries, walk paragraphs in order, after the Nth </p> emit any marginalia entries whose paragraph == N+1 as inline <details> mobile copies (canonical), AND emit a sibling .marginalia-rail aside containing the desktop copies (with aria-hidden="true"). Each desktop note carries an inline style with --marginalia-rot computed deterministically from a stable hash of the date. When a marginalia entry's paragraph index exceeds the number of paragraphs in the body, drop it and emit a warnf containing the post path, bad index, and actual paragraph count.

## Design

Use Hugo's split function on "</p>" and the where function to filter marginalia by paragraph index. Paragraph counter is the loop index + 1; iterate only the first len(parts)-1 elements (the last split-piece has no closing </p>). Deterministic rotation: hash := mod (printf "%s" .date | strings.SHA1 | first 8 ...) — actually simpler: take a numeric hash via printf and modulo into [-2, 2] via int conversion. Or use mod (len .date) 5 - 2 as a trivial deterministic mapping (date strings are length 10 — trivial; pick something with more entropy like sum of char codes). Mark the rail with aria-hidden="true" so screen readers see only the inline copy. Theme files under themes/congo/ MUST NOT be modified — the override lives entirely at layouts/_default/single.html.

## Acceptance Criteria

layouts/_default/single.html exists. Posts under content/blog/ and content/journal/<project>/ with non-empty .Params.marginalia render the configured notes per the marginalia capability spec. Posts without marginalia render byte-identically (modulo whitespace) to before. Out-of-range paragraph indices are dropped and trigger a build-time warnf naming the post + bad index + actual count. Desktop rail carries aria-hidden="true". Same post built twice produces identical rotation values per note. Pages whose Section is not blog or journal ignore the marginalia front-matter entirely. themes/congo/ unmodified.

