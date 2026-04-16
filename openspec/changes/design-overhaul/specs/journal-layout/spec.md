## ADDED Requirements

### Requirement: Journal section replaces Research
The system SHALL serve journal content at `/journal/` instead of `/research/`. All content previously under `content/research/` SHALL exist under `content/journal/` with the same project-based structure: `content/journal/<project>/_index.md` and `content/journal/<project>/<entry>.md`.

#### Scenario: Research URLs redirect
- **WHEN** building the site after the rename
- **THEN** `hugo --minify` exits 0 with no broken `ref` errors
- **THEN** journal content is accessible at `/journal/` paths

### Requirement: Box-grid list layout
The Journal landing page (`content/journal/_index.md`) SHALL display projects as boxes in a responsive grid layout.

#### Scenario: Desktop grid
- **WHEN** viewing `/journal/` on a viewport ≥ 1024px wide
- **THEN** project boxes display 3 per row

#### Scenario: Tablet grid
- **WHEN** viewing `/journal/` on a viewport between 640px and 1023px
- **THEN** project boxes display 2 per row

#### Scenario: Mobile grid
- **WHEN** viewing `/journal/` on a viewport < 640px
- **THEN** project boxes display 1 per row (stacked)

### Requirement: Project box content
Each project box SHALL display the project title and a brief description (from the project `_index.md` frontmatter `description` field). If a `thumbnail` param is set, it SHALL also display the thumbnail image.

#### Scenario: Box displays project info
- **WHEN** a project exists at `content/journal/example-project/_index.md` with a title and description
- **THEN** the journal grid shows a box with that title and description

### Requirement: Project box styling
Project boxes SHALL use the custom color scheme: warm cream background, burnt orange border or accent, green title text.

#### Scenario: Box visual identity
- **WHEN** viewing the journal grid
- **THEN** boxes have a visible border or accent using burnt orange and titles render in forest green

### Requirement: Box links to project
Clicking a project box SHALL navigate to the project landing page (e.g., `/journal/example-project/`), which lists entries chronologically.

#### Scenario: Box click navigation
- **WHEN** clicking a project box
- **THEN** the browser navigates to the project's `_index.md` page

### Requirement: Cross-link updates
All internal `ref` and `relref` shortcodes that previously pointed to `/research/...` SHALL be updated to point to `/journal/...`.

#### Scenario: Blog cross-links to journal
- **WHEN** a blog post links to a journal entry via `ref "/journal/example-project/example-entry"`
- **THEN** the link resolves correctly and `hugo --minify` exits 0
