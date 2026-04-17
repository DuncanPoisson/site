# Delta for Homepage Layout

## ADDED Requirements

### Requirement: Custom home template
The root page (`/`) SHALL be rendered by a project-level Hugo layout override (`layouts/index.html`) that Hugo resolves ahead of the Congo theme's default home template. The layout SHALL NOT modify any file under `themes/congo/`.

#### Scenario: Project layout takes precedence
- **WHEN** `hugo --minify` builds the site and `public/index.html` is read
- **THEN** its markup reflects the custom four-section structure defined below (hero + research questions + bio + quote), not Congo's default home output

#### Scenario: Theme directory is untouched
- **WHEN** `git status` is inspected against `themes/congo/`
- **THEN** no modifications, new files, or deletions are reported inside that directory

### Requirement: Four sections separated by burnt orange dividers
The homepage SHALL contain exactly four top-level sections in this order: (1) Hero, (2) What is this website?, (3) Who am I?, (4) Quote. Each section SHALL be followed by an `<hr>` element styled by the existing `visual-dividers` rule (burnt orange `#C74D20`, 2–3px solid).

#### Scenario: Sections and dividers are present
- **WHEN** the rendered `/` markup is inspected
- **THEN** it contains, in order, a hero block, an `<hr>`, a "What is this website?" block, an `<hr>`, a "Who am I?" block, an `<hr>`, and a quote block

### Requirement: Hero section
The Hero section SHALL display Duncan's photo (sourced from `static/images/duncan.jpg`) and an introductory text block containing, at minimum, the heading `Duncan Xavier Haddock` rendered in the primary green color (`#429347`). On viewports wider than 768px the photo SHALL appear to the left of the text; on viewports 768px or narrower the photo SHALL appear above the text.

#### Scenario: Photo asset is referenced
- **WHEN** the rendered home page is inspected
- **THEN** an `<img>` element references `/images/duncan.jpg` (or an equivalent Hugo-processed URL to that asset)

#### Scenario: Name heading uses primary green
- **WHEN** the `Duncan Xavier Haddock` heading is inspected in a browser
- **THEN** its computed `color` resolves to the primary green (`#429347`)

#### Scenario: Responsive stacking
- **WHEN** the viewport width is less than or equal to 768px
- **THEN** the photo is positioned above the introduction text in a single-column layout

### Requirement: "What is this website?" section with three section cards
The second section SHALL render a heading in the primary green (`#429347`), a list of research questions as bullet items, the transition sentence `This site is where I work on these questions... in three parts!`, and then exactly three clickable cards arranged in a row on desktop and stacked on mobile. Each card SHALL be an anchor element (`<a>`) styled as a rounded rectangle (border-radius 8–12px) with background `#429347` and white text, each containing a bold title and a subtitle.

Cards SHALL appear in this order with these contents and link targets:
1. Title `Blog` → href `/blog/` → subtitle `Exploratory thinking. Responses to readings, connections between ideas, reflections on talks and local issues.`
2. Title `Journal` → href `/journal/` → subtitle `Analytical work. Reproducing results from papers, working through datasets, building models, and question development.`
3. Title `Projects` → href `/projects/` → subtitle `Public work I have contributed to.`

#### Scenario: Cards render as clickable anchors
- **WHEN** the section is inspected
- **THEN** each of the three cards is an `<a>` element whose entire rectangular area is the click target (single anchor per card)

#### Scenario: Card link targets are correct
- **WHEN** the three cards are read in document order
- **THEN** their `href` attributes resolve to `/blog/`, `/journal/`, and `/projects/` respectively

#### Scenario: Responsive card layout
- **WHEN** the viewport width is greater than 768px
- **THEN** the three cards are laid out in a single row of equal-width columns
- **WHEN** the viewport width is less than or equal to 768px
- **THEN** the three cards are stacked in a single column

### Requirement: "Who am I?" section with external-link card
The third section SHALL render a heading in the primary green (`#429347`), the biographical body paragraphs, and below the text exactly one clickable card styled identically to the Blog/Journal/Projects cards (rounded green rectangle, white text). The card's href SHALL be `https://linktr.ee/` (a placeholder URL flagged as TODO in the source) and its visible text SHALL be a variant of `More of me`.

#### Scenario: Single external card renders
- **WHEN** the section is inspected
- **THEN** it contains exactly one `<a>` card with an absolute URL starting with `https://linktr.ee/` and label text beginning with `More of me`

#### Scenario: Card styling matches section-card style
- **WHEN** the card's computed background color is inspected
- **THEN** it resolves to `#429347` and the text color resolves to white

### Requirement: Closing quote
The fourth section SHALL render the following text, centered, as an italic blockquote at a slightly larger font size than body text, with visible attribution to Angela Davis:

> I am no longer accepting the things I cannot change. I am changing the things I cannot accept.
>
> — Angela Davis

#### Scenario: Quote markup and alignment
- **WHEN** the section is inspected
- **THEN** the quote text sits inside a `<blockquote>` (or equivalent semantic element), is horizontally center-aligned, renders in italic style at a font size larger than surrounding paragraph text, and includes the attribution `Angela Davis`

### Requirement: Final homepage copy
`content/_index.md` SHALL carry the final authored copy provided in the project spec (hero introduction, research-questions body, bio body) verbatim — no rewording, paraphrasing, or added/removed sentences — and its front matter SHALL set `title` to `Duncan Xavier Haddock`.

#### Scenario: Copy is verbatim
- **WHEN** `content/_index.md` is read
- **THEN** it contains, in order and without alteration, the hero paragraphs, the "I find myself with many questions…" bulleted block through the `in three parts!` transition sentence, and the "By training, I am a physicist…" biographical paragraphs as specified

#### Scenario: Front matter title is correct
- **WHEN** the front matter of `content/_index.md` is parsed
- **THEN** the `title` key equals `Duncan Xavier Haddock`

### Requirement: Photo placeholder asset
A file SHALL exist at `static/images/duncan.jpg` so the built site references a valid image path even before the user supplies a final photograph.

#### Scenario: Placeholder file exists
- **WHEN** `static/images/duncan.jpg` is checked on disk
- **THEN** the file exists and is a valid image readable by Hugo's image pipeline (or a plain JPG file served as-is from `static/`)

### Requirement: Clean build
The site SHALL build without errors after these changes. `hugo --minify` SHALL exit with status code `0` and its stdout/stderr SHALL contain no lines beginning with `ERROR`.

#### Scenario: Build succeeds
- **WHEN** `hugo --minify` is executed from the project root
- **THEN** the command exits with status `0` and its combined output contains no `ERROR` lines

### Requirement: Preserved local overrides
This change SHALL NOT remove or alter `layouts/partials/functions/warnings.html`.

#### Scenario: Warnings partial still present
- **WHEN** the repository is inspected after the change is applied
- **THEN** `layouts/partials/functions/warnings.html` exists and its contents are byte-for-byte identical to the version on `main` prior to this change
