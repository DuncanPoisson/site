# Delta for homepage-layout

## MODIFIED Requirements

### Requirement: Hero section
The Hero section SHALL display Duncan's photo (sourced from `static/images/duncan.jpg`) and an introductory text block containing, at minimum, the heading `Duncan Xavier Haddock` rendered in the primary green color (`#082620`). On viewports wider than 768px the photo SHALL appear to the left of the text; on viewports 768px or narrower the photo SHALL appear above the text.

(Previously: heading color `#429347`.)

#### Scenario: Photo asset is referenced
- **WHEN** the rendered home page is inspected
- **THEN** an `<img>` element references `/images/duncan.jpg` (or an equivalent Hugo-processed URL to that asset)

#### Scenario: Name heading uses primary green
- **WHEN** the `Duncan Xavier Haddock` heading is inspected in a browser
- **THEN** its computed `color` resolves to the primary green (`#082620`)

#### Scenario: Responsive stacking
- **WHEN** the viewport width is less than or equal to 768px
- **THEN** the photo is positioned above the introduction text in a single-column layout

### Requirement: "What is this Website?" section with matrix and three section cards
The second section SHALL render a heading `What is this Website?` in the primary green (`#082620`), followed by a two-row matrix block, followed by exactly three clickable section cards in a row on desktop (stacked on mobile).

Matrix layout:
- **Row 1** SHALL place a text block on the LEFT (occupying 2/5 of the container width on desktop) and an image `<img>` sourced from `static/images/photo1.png` on the RIGHT (occupying 3/5 of the container width on desktop). The text content SHALL be verbatim:

  > This site is where I catalog my reading and learning about the complex world around us. My current interests sit at the intersection of urban systems and the structural changes a sustainable society would demand — what a green energy transition would require of labor and existing infrastructure, how urban design can foster community rather than atomize it, and what incentives might push cities toward more holistic approaches to production, consumption, and waste. These threads pull on each other more than they stand apart, and part of what I want to do here is trace those connections.

- **Row 2** SHALL place an image `<img>` sourced from `static/images/photo2.png` on the LEFT (3/5 of container width on desktop) and a text block on the RIGHT (2/5 of container width on desktop). The text content SHALL be verbatim:

  > How do we effectively model these changes? What happens to individual behavior when you put a grocery store in a neighborhood, pull a lane of traffic for a bike lane, or shift to more sustainable construction materials — and how do those changes scale? This site is where I work through those questions, using three mechanisms:

- On viewports ≤ 768px each row SHALL collapse into a single column. Row 1 SHALL display text above photo; row 2 SHALL display photo above text.

Below the matrix, three clickable cards SHALL appear in order with these contents and link targets:
1. Title `Blog` → href `/blog/` → subtitle `Exploratory thinking. Responses to readings, connections between ideas, reflections on talks and local issues.`
2. Title `Journal` → href `/journal/` → subtitle `Analytical work. Reproducing results from papers, working through datasets, building models.`
3. Title `Projects` → href `/projects/` → subtitle `A place to house designs and questions I'm working through.`

Each card SHALL be an anchor element (`<a>`) styled as a rounded rectangle (border-radius 8–12px) with background `#082620` and text color `#EAF9E1`, containing a centered title and a left-aligned description.

(Previously: heading lower-case `w` in "website"; body was a bulleted research-questions list and a transition sentence; Journal subtitle ended with "and question development"; Projects subtitle read "Public work I have contributed to.")

#### Scenario: Heading casing
- **WHEN** the section heading is inspected
- **THEN** it reads exactly `What is this Website?` (capital `W` in `Website`)

#### Scenario: Matrix row 1 layout and content
- **WHEN** the rendered page is inspected on a viewport > 768px
- **THEN** row 1 of the matrix places the text block before the `<img src=".../photo1.png">` in a two-column grid or equivalent, with the text column narrower than the photo column
- **THEN** the text content matches the verbatim row-1 copy defined above

#### Scenario: Matrix row 2 layout and content
- **WHEN** the rendered page is inspected on a viewport > 768px
- **THEN** row 2 of the matrix places the `<img src=".../photo2.png">` before the text block, with the photo column wider than the text column
- **THEN** the text content matches the verbatim row-2 copy defined above

#### Scenario: Matrix responsive collapse
- **WHEN** the viewport width is ≤ 768px
- **THEN** each matrix row stacks vertically
- **THEN** in row 1 the text appears above the photo
- **THEN** in row 2 the photo appears above the text

#### Scenario: Cards render as clickable anchors below the matrix
- **WHEN** the section is inspected
- **THEN** each of the three cards is an `<a>` element whose entire rectangular area is the click target (single anchor per card)
- **THEN** the three cards appear AFTER the matrix block in document order

#### Scenario: Card link targets and copy are correct
- **WHEN** the three cards are read in document order
- **THEN** their `href` attributes resolve to `/blog/`, `/journal/`, and `/projects/` respectively
- **THEN** their title and subtitle text matches the bullets above verbatim

#### Scenario: Card visual style
- **WHEN** a card is inspected in a browser
- **THEN** its computed background-color resolves to `rgb(8, 38, 32)` (`#082620`)
- **THEN** its title and subtitle text color resolves to `rgb(234, 249, 225)` (`#EAF9E1`)
- **THEN** the title text is horizontally center-aligned
- **THEN** the subtitle text is left-aligned
- **THEN** no list-marker (bullet, disc, number) is visible next to the card

#### Scenario: Responsive card layout
- **WHEN** the viewport width is greater than 768px
- **THEN** the three cards are laid out in a single row of equal-width columns
- **WHEN** the viewport width is less than or equal to 768px
- **THEN** the three cards are stacked in a single column

### Requirement: "Who am I?" section with external-link card
The third section SHALL render a heading in the primary green (`#082620`), the biographical body paragraphs, and below the text exactly one clickable card styled identically to the Blog/Journal/Projects cards (rounded green rectangle with background `#082620` and text color `#EAF9E1`). The card's href SHALL be `https://linktr.ee/duncanpoisson` and its visible text SHALL be a variant of `More of Me`.

The single card SHALL be horizontally centered within the section, SHALL occupy between 50% and 60% of the section's container width on viewports > 768px, and its text SHALL be horizontally center-aligned within the card. On viewports ≤ 768px the card SHALL collapse to full (or near-full) width, with text remaining center-aligned.

(Previously: heading color `#429347`; card href was placeholder `https://linktr.ee/` flagged TODO; card width matched the three-column card width; card text was left-aligned; card was not explicitly centered.)

#### Scenario: Single external card renders with real URL
- **WHEN** the section is inspected
- **THEN** it contains exactly one `<a>` card with `href="https://linktr.ee/duncanpoisson"` and label text beginning with `More of Me`

#### Scenario: Card is centered and widened
- **WHEN** the section is inspected on a viewport > 768px
- **THEN** the card is horizontally centered within its container
- **THEN** its rendered width is between 50% and 60% of the container width
- **THEN** its title text is horizontally center-aligned within the card

#### Scenario: Card styling matches section-card style
- **WHEN** the card's computed background color is inspected
- **THEN** it resolves to `rgb(8, 38, 32)` (`#082620`) and the text color resolves to `rgb(234, 249, 225)` (`#EAF9E1`)

#### Scenario: No list-marker visible
- **WHEN** the card is inspected
- **THEN** no list-marker (bullet, disc, number) appears next to it, regardless of whether the containing element is a `<ul>` or `<ol>`

### Requirement: Final homepage copy
`content/_index.md` SHALL carry the final authored copy — verbatim, with no rewording, paraphrasing, or added/removed sentences — and its front matter SHALL set `title` to `Duncan Xavier Haddock`. The hero introduction and the bio body SHALL remain byte-for-byte identical to the prior `custom-homepage` content. The two matrix-section body blocks SHALL match the verbatim text defined in the "What is this Website?" requirement above. The three bubble subtitles and the `More of Me` card label SHALL match the text defined above verbatim.

(Previously: the section-2 body was a bulleted research-questions list.)

#### Scenario: Hero and bio are preserved verbatim
- **WHEN** `content/_index.md` is read
- **THEN** the hero introductory paragraphs and the bio body paragraphs are byte-for-byte identical to their values as of the `custom-homepage` change

#### Scenario: Section-2 matrix text is verbatim
- **WHEN** `content/_index.md` (or `layouts/index.html`, if authored copy is inlined there) is read
- **THEN** it contains the two row-blocks' text verbatim, each as a single paragraph, without paraphrase

#### Scenario: Front matter title is correct
- **WHEN** the front matter of `content/_index.md` is parsed
- **THEN** the `title` key equals `Duncan Xavier Haddock`

## ADDED Requirements

### Requirement: Bubble list-marker reset
The homepage SHALL NOT render default HTML list markers (disc, bullet, number) beside any `.home-card` bubble, regardless of whether the cards are wrapped in a `<ul>`, `<ol>`, or plain containers. The `list-style: none` reset (or equivalent) SHALL live in `layouts/index.html` inline styles or `assets/css/custom.css`, NOT in `themes/congo/`.

#### Scenario: No markers on the three-card group
- **WHEN** the rendered page is inspected
- **THEN** no visible list markers appear next to the Blog, Journal, or Projects cards

#### Scenario: No markers on the More-of-Me card
- **WHEN** the rendered page is inspected
- **THEN** no visible list marker appears next to the `More of Me` card

### Requirement: Section-2 photo assets present
Files SHALL exist at `static/images/photo1.png` and `static/images/photo2.png` so the matrix rows reference valid image paths at build time.

#### Scenario: Photo assets exist
- **WHEN** `static/images/photo1.png` and `static/images/photo2.png` are checked on disk
- **THEN** both files exist and are valid images readable by Hugo's image pipeline (or plain PNGs served as-is from `static/`)
