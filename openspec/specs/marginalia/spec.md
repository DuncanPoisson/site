# marginalia Specification

## Purpose
TBD - created by archiving change add-marginalia. Update Purpose after archive.
## Requirements
### Requirement: Front-matter authoring shape
A post SHALL declare marginalia in its front-matter under a top-level `marginalia` list. Each list entry SHALL have the fields `paragraph` (integer ≥ 1), `date` (ISO-8601 calendar date), and `note` (string, markdown-formatted). No other fields are recognized in v1; unknown fields are ignored.

#### Scenario: Valid marginalia front-matter
- **WHEN** a blog post's front-matter contains `marginalia: [{paragraph: 3, date: 2027-11-03, note: "I'd put this differently now."}]`
- **THEN** `hugo --minify` exits 0 with no `ERROR` lines
- **THEN** the rendered post page contains a marginal note keyed to the third paragraph with the text "I'd put this differently now."

#### Scenario: Empty or absent marginalia
- **WHEN** a post has no `marginalia` key, or `marginalia: []`
- **THEN** the rendered post page is byte-identical (modulo whitespace) to the same post built without the feature

#### Scenario: Out-of-range paragraph index
- **WHEN** a marginalia entry references a `paragraph` greater than the count of paragraphs in the rendered body
- **THEN** the entry is omitted from the rendered output
- **THEN** `hugo --minify` emits a `warnf` containing the post path, the bad index, and the actual paragraph count

### Requirement: Paragraph definition
For the purpose of paragraph indexing, a "paragraph" SHALL be defined as the Nth occurrence of `</p>` in the post's rendered HTML body, counting from 1. Other block elements (lists, blockquotes, code blocks, headings, images, tables) SHALL NOT increment the paragraph counter.

#### Scenario: Headings and lists do not count
- **WHEN** a post body is `# Heading\n\nFirst paragraph.\n\n- item 1\n- item 2\n\nSecond paragraph.`
- **THEN** a marginalia entry with `paragraph: 2` targets "Second paragraph."

### Requirement: Desktop rail rendering
On viewports ≥ 1024px, marginalia SHALL render in a right-side gutter alongside the article body. Each note SHALL display as a small block containing a date stamp and the note body, styled with a handwriting-family font, a sticky-note-tone background derived from the secondary color scale, and a small deterministic rotation in the range [-2deg, +2deg] computed from a stable hash of the note's date.

#### Scenario: Desktop layout
- **WHEN** a viewport is ≥ 1024px and a post has at least one marginalia entry
- **THEN** the article body renders in its normal `max-w-prose` column
- **THEN** a sibling rail renders to the right of the article column containing each note
- **THEN** each rendered note shows its date in the format `YYYY-MM-DD` and its note body rendered as markdown

#### Scenario: Deterministic rotation
- **WHEN** the same post is built twice without changing its marginalia
- **THEN** each note's rotation angle is identical across both builds

### Requirement: Mobile inline rendering
On viewports < 1024px, marginalia SHALL render inline as `<details>` elements immediately after their target paragraph. The `<summary>` SHALL contain the date in `YYYY-MM-DD` format. The `<details>` body SHALL contain the note rendered as markdown.

#### Scenario: Mobile layout
- **WHEN** a viewport is < 1024px and a post has marginalia
- **THEN** each note renders as a collapsed `<details>` immediately after its target paragraph in DOM order
- **THEN** the desktop rail (if present in DOM) is hidden via CSS

### Requirement: Accessibility and DOM order
The mobile inline copy of each note SHALL be the canonical copy in the DOM. The desktop rail copy SHALL carry `aria-hidden="true"` so assistive tech reads only the inline copy regardless of viewport.

#### Scenario: Screen reader sees inline copy
- **WHEN** a post is rendered and inspected as text
- **THEN** each note's text appears exactly once in the accessibility tree (via the inline `<details>` copy)

### Requirement: Self-hosted handwriting font
The site SHALL ship a handwriting webfont locally under `static/fonts/`. An `@font-face` declaration in `assets/css/custom.css` SHALL declare the family with `font-display: swap`. No external font requests SHALL be made at runtime to render marginalia.

#### Scenario: No third-party font requests
- **WHEN** a built page containing marginalia is loaded
- **THEN** the page makes no network request to fonts.googleapis.com, fonts.gstatic.com, or any other third-party font host

### Requirement: Print rendering
When printed, marginalia SHALL render inline-after-paragraph (the mobile variant), and the desktop rail SHALL be hidden.

#### Scenario: Print stylesheet
- **WHEN** a post is printed (or print-previewed)
- **THEN** each note appears inline after its target paragraph, expanded
- **THEN** no rail or duplicate rendering appears

### Requirement: Scope limited to blog and journal entries
Marginalia SHALL render only for pages whose `Section` is `blog` or `journal`. Other sections (`gallery`, `about`, `cv`, `projects`) SHALL ignore the front-matter key entirely, even if present.

#### Scenario: Marginalia in About is ignored
- **WHEN** `content/about/_index.md` declares a `marginalia` front-matter list
- **THEN** the rendered About page contains no marginalia output

### Requirement: Theme files unmodified
The implementation SHALL NOT modify any file under `themes/congo/`. All overrides SHALL live in the project's `layouts/`, `assets/`, `static/`, and `content/` trees.

#### Scenario: No theme diff
- **WHEN** `git diff` is run against the `themes/congo/` submodule pointer
- **THEN** no files inside `themes/congo/` are modified

### Requirement: Example marginalia ship with the site
At least one blog post and at least one journal entry SHALL ship with example marginalia in front-matter so the feature is observable on a fresh build.

#### Scenario: Examples present
- **WHEN** the site is built fresh from main after this change lands
- **THEN** `/blog/` contains at least one post whose rendered page shows marginalia
- **THEN** `/journal/` contains at least one entry whose rendered page shows marginalia

