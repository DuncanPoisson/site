# gallery Specification

## Purpose
TBD - created by archiving change design-refinement. Update Purpose after archive.
## Requirements
### Requirement: Gallery nav item
The site's primary navigation SHALL include a `Gallery` menu entry that links to `/gallery/`, positioned between `Journal` and `Projects`.

#### Scenario: Nav order
- **WHEN** the primary nav is rendered on any page
- **THEN** its items appear in this order: About, Blog, Journal, Gallery, Projects

#### Scenario: Gallery link target
- **WHEN** the `Gallery` nav item is clicked
- **THEN** the browser navigates to the site-relative URL `/gallery/`

### Requirement: Gallery landing page lists collections as cards
The `/gallery/` page SHALL render a responsive grid of collection cards — three columns on viewports ≥ 1024px, two columns on viewports ≥ 640px and < 1024px, one column on viewports < 640px. Each card SHALL correspond to one gallery collection (a subdirectory of `content/gallery/`) and SHALL display at minimum the collection's `title` and `description`.

#### Scenario: Cards reflect on-disk collections
- **WHEN** `content/gallery/<slug>/_index.md` exists
- **THEN** the `/gallery/` page renders a card whose title matches the `title` front-matter and whose description matches the `description` front-matter
- **THEN** clicking the card navigates to `/gallery/<slug>/`

#### Scenario: Responsive grid
- **WHEN** the viewport is ≥ 1024px wide
- **THEN** collection cards lay out in three equal-width columns
- **WHEN** the viewport is between 640px and 1024px wide
- **THEN** collection cards lay out in two equal-width columns
- **WHEN** the viewport is < 640px wide
- **THEN** collection cards stack in a single column

### Requirement: Collection page renders a photo grid
A gallery collection page at `/gallery/<slug>/` SHALL render every image resource attached to the collection's `_index.md` (via Hugo's page-bundle `Resources`) in a responsive grid beneath the collection title and description.

#### Scenario: Images render as a grid
- **WHEN** the collection's `_index.md` has image resources attached (e.g., `photo-a.jpg`, `photo-b.jpg`)
- **THEN** the `/gallery/<slug>/` page renders an `<img>` for each image resource
- **THEN** images are arranged in a multi-column responsive grid (at least two columns on viewports ≥ 640px, collapsing to one column on narrower viewports)

#### Scenario: Empty-state message
- **WHEN** a collection has zero image resources
- **THEN** the collection page renders a human-readable empty-state message (e.g., "No photos yet.") instead of an empty grid or a build error

### Requirement: Example collection committed
The repository SHALL include at least one committed gallery collection under `content/gallery/` (with populated front-matter: `title`, `description`) so the `/gallery/` landing page renders a non-empty grid on a fresh build.

#### Scenario: Example collection exists
- **WHEN** `content/gallery/` is inspected
- **THEN** it contains `_index.md` and at least one subdirectory with its own `_index.md` carrying `title` and `description` front-matter

### Requirement: Custom layouts live outside the theme
The gallery capability SHALL be implemented via project-level Hugo layouts at `layouts/gallery/list.html` and `layouts/gallery/single.html` (or equivalent project-level paths Hugo resolves ahead of the theme). No file under `themes/congo/` SHALL be modified.

#### Scenario: Layouts in project root
- **WHEN** the repository is inspected
- **THEN** `layouts/gallery/list.html` and a per-collection single layout exist under `layouts/` (not under `themes/congo/`)

#### Scenario: Theme untouched
- **WHEN** `git status` is inspected against `themes/congo/`
- **THEN** no modifications are reported

### Requirement: CV remains reachable
Removing `CV` from the navigation SHALL NOT remove or rename `content/cv/`. The URL `/cv/` SHALL continue to resolve to the CV page.

#### Scenario: /cv/ still builds
- **WHEN** `hugo --minify` builds the site
- **THEN** `public/cv/index.html` is generated
- **THEN** its content is the same CV page that existed before this change

