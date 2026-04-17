---
id: sit-jdig
status: open
deps: [sit-h2q2]
links: []
created: 2026-04-17T23:10:32Z
type: task
priority: 1
assignee: Duncan Haddock
external-ref: openspec:design-refinement
parent: sit-t34p
tags: [design, layout, content, homepage]
---
# Homepage section 2 matrix + bubble restyle (layouts/index.html, content/_index.md)

Rewrite the homepage middle section as a two-row photo/text matrix and restyle all bubbles. In layouts/index.html inline <style>: add .home-matrix with two rows (row 1 grid-template-columns 2fr 3fr, row 2 3fr 2fr, with gap and responsive image sizing), update .home-card to background #082620 and color #EAF9E1, center .home-card-title while leaving .home-card-subtitle left-aligned, add list-style: none on .home-cards and .home-cards-single, add a .home-card-wide modifier (50-60% width, horizontally centered, text-align center), and update every remaining #429347 reference in the style block to #082620. In markup: change the section-2 heading text to 'What is this Website?' (capital W), replace the {{ .Params.questions_body | markdownify }} block with two matrix rows (row 1 text-left + <img src=.../photo1.png> right; row 2 <img src=.../photo2.png> left + text-right), keep the three bubbles but position them below the matrix, update the three bubble subtitles (Blog: 'Exploratory thinking. Responses to readings, connections between ideas, reflections on talks and local issues.'; Journal: 'Analytical work. Reproducing results from papers, working through datasets, building models.'; Projects: 'A place to house designs and questions I'm working through.'), and update the More-of-Me card to href='https://linktr.ee/duncanpoisson' with the .home-card-wide modifier. In content/_index.md: remove the questions_body param, add the two matrix-row paragraphs verbatim (either as new params or inlined in layouts/index.html — pick whichever reads cleaner), keep hero_intro and bio_body byte-for-byte identical. Keep the hero section, who-am-I heading text, and quote block unchanged.

## Design

Matrix on desktop: CSS grid with 2fr/3fr for row 1 and 3fr/2fr for row 2 — photos get the larger share since they carry more visual information. Below 768px: each row collapses to a single column; row 1 places text above photo (source order), row 2 places photo above text (source order), consistent with the spec. Images size as width: 100%, height: auto, border-radius matching hero image (~12px). Inline <style> follows the precedent set by the prior custom-homepage change. The three bubbles stay in a <ul class='home-cards'> for semantic grouping; list-style: none removes the markers. The More-of-Me card stays in a <ul class='home-cards-single'> and picks up .home-card-wide for the centered, widened treatment. Verbatim section-2 copy (from the project spec): Row 1 = 'This site is where I catalog my reading and learning about the complex world around us. My current interests sit at the intersection of urban systems and the structural changes a sustainable society would demand — what a green energy transition would require of labor and existing infrastructure, how urban design can foster community rather than atomize it, and what incentives might push cities toward more holistic approaches to production, consumption, and waste. These threads pull on each other more than they stand apart, and part of what I want to do here is trace those connections.' Row 2 = 'How do we effectively model these changes? What happens to individual behavior when you put a grocery store in a neighborhood, pull a lane of traffic for a bike lane, or shift to more sustainable construction materials — and how do those changes scale? This site is where I work through those questions, using three mechanisms:'

## Acceptance Criteria

Section-2 heading text is exactly 'What is this Website?' (capital W). Rendered section 2 on a >768px viewport shows row 1 text-left + photo1.png right (text narrower than photo) and row 2 photo2.png left + text-right (photo wider than text); on a ≤768px viewport row 1 stacks text-above-photo and row 2 stacks photo-above-text. The verbatim row-1 and row-2 paragraphs appear in document order. Below the matrix, three <a class='home-card'> cards link to /blog/, /journal/, /projects/ with the updated subtitles; computed background is rgb(8, 38, 32), computed text color rgb(234, 249, 225), title is center-aligned, subtitle is left-aligned, and no list-marker (disc/bullet/number) is visible next to any card. The More-of-Me card href is exactly 'https://linktr.ee/duncanpoisson', card is horizontally centered within its section, its width is 50-60% of container on desktop, and its text is center-aligned. hero_intro and bio_body in content/_index.md are byte-for-byte identical to the prior commit. themes/congo/ unmodified. hugo --minify exits 0 with zero ERROR lines.

