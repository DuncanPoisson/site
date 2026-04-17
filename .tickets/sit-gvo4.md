---
id: sit-gvo4
status: closed
deps: [sit-gp18]
links: []
created: 2026-04-16T23:39:52Z
type: task
priority: 1
assignee: Duncan Haddock
external-ref: openspec:custom-homepage
parent: sit-3tct
tags: [design, layout, content]
---
# Custom homepage layout with four sections and final copy

Create layouts/index.html as a project-level Hugo override for the root page. Render exactly four sections separated by <hr> dividers (burnt orange styling already in assets/css/custom.css): (1) Hero with photo + 'Duncan Xavier Haddock' heading in primary green + intro paragraphs; (2) 'What is this website?' with green heading, bulleted research questions, transition sentence, and three clickable green cards linking /blog/, /journal/, /projects/ with exact titles and subtitles from the spec; (3) 'Who am I?' with green heading, bio paragraphs, and one 'More of me' green card linking https://linktr.ee/ (TODO URL); (4) Centered italic Angela Davis blockquote with attribution. Replace content/_index.md with the final authored copy verbatim per the project spec; set front matter title = 'Duncan Xavier Haddock' and defensively disable Congo article flags (showDate, showReadingTime, showTableOfContents, showBreadcrumbs).

## Design

Layout override path: layouts/index.html (top-level) — narrower than layouts/_default/home.html. Inline <style> block co-located with markup (matches the design-overhaul precedent). Hero: CSS Grid, photo column ~280px left, text right; collapse to single column with photo on top at max-width 768px. Card row: grid-template-columns repeat(3, 1fr), gap ~1rem, collapses to single column at 768px. Cards: semantic <a class='home-card'> anchors styled as blocks — background #429347, color white, border-radius 8-12px, padding ~1.25rem, bold title + subtitle. 'More of me' card uses the same home-card class. Put bulleted questions inline in the layout (or sourced from _index.md markdown .Content — decide at implementation: simplest is markdown .Content for body paragraphs + hardcoded headings/cards in the layout). Photo <img> references /images/duncan.jpg with alt text like 'Portrait of Duncan'. Angela Davis quote rendered as <blockquote> centered, italic, font-size ~1.125-1.25rem, with attribution on its own line prefixed by an em dash. Copy must match the project spec WORD FOR WORD — do not rewrite.

## Acceptance Criteria

layouts/index.html exists and renders the root page with the four sections in order, each followed by <hr> (except possibly the last). Hero photo sits left of text at >768px and stacks above at <=768px. Hero name heading 'Duncan Xavier Haddock' computed color resolves to #429347. Three section cards in document order with hrefs /blog/, /journal/, /projects/; each is a single <a>; subtitle text matches spec verbatim. Fourth card in Who-am-I section is a single <a> with href starting https://linktr.ee/ and label beginning 'More of me'. All four cards computed background #429347, color white, border-radius between 8px and 12px. Quote wrapped in <blockquote>, center-aligned, italic, with 'Angela Davis' attribution. content/_index.md body copy matches spec verbatim (hero intro, research-questions body, bio paragraphs). themes/congo/ files unmodified. layouts/partials/functions/warnings.html unmodified. hugo --minify exits 0 with no ERROR lines.


## Notes

**2026-04-17T17:57:14Z**

Created layouts/index.html defining block 'main', with inline <style> for hero grid + card grid (768px breakpoint), green name heading (#429347), green home-card anchors with white text and 10px border-radius, and a centered italic blockquote. content/_index.md stores hero_intro, questions_body, and bio_body as YAML block scalars (verbatim copy); rendered via {{ markdownify }}. Verified public/index.html: hero <section>, 3 <hr> dividers, three home-card anchors to /blog/, /journal/, /projects/, single 'More of me' card to https://linktr.ee/, and blockquote+cite for Angela Davis. hugo --minify exits 0.
