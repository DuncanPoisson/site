# Delta for site-architecture

## ADDED Requirements

### Requirement: Top-level navigation
The site SHALL expose a top-level navigation menu containing links to Home, Blog, Research Journal, CV, and About, configured via `config/_default/menus.toml`.

#### Scenario: Menu renders on every page
- **WHEN** any rendered page of the site is loaded
- **THEN** the Congo header shows menu items "Home", "Blog", "Research Journal", "CV", "About" in that order, each linking to the correct section under the `/site/` baseURL

#### Scenario: Menu items link within the subpath
- **WHEN** a user clicks a menu item
- **THEN** the destination URL begins with `https://duncanpoisson.github.io/site/` and does not 404

### Requirement: Homepage layout
The site SHALL render `content/_index.md` using Congo's `page` homepage layout with a brief placeholder statement of direction (2-3 sentences clearly marked as placeholder) and a link to the About page.

#### Scenario: Homepage loads
- **WHEN** a user visits the site root
- **THEN** the homepage renders without errors, shows placeholder text, and includes a link to `/about/`

### Requirement: Blog section
The site SHALL provide a Blog section at `content/blog/` whose list page presents posts in reverse chronological order, with each post supporting `title`, `date`, `draft`, `tags`, `categories`, and `description` frontmatter fields.

#### Scenario: Post listing is date-ordered
- **WHEN** multiple posts exist in `content/blog/`
- **THEN** the `/blog/` list page renders them newest-first

#### Scenario: Example post renders with metadata
- **WHEN** the example blog post is rendered
- **THEN** the page displays an estimated reading time and a table of contents generated from its headings

### Requirement: Research Journal section, project-indexed
The site SHALL organize the Research Journal by project under `content/research/<project>/<entry>.md`, with `content/research/_index.md` listing all projects and each `content/research/<project>/_index.md` listing its entries in reverse chronological order.

#### Scenario: Research landing lists projects
- **WHEN** a user visits `/research/`
- **THEN** the page lists each project with its description from the project's `_index.md`

#### Scenario: Project landing lists entries
- **WHEN** a user visits `/research/example-project/`
- **THEN** the page lists entries in reverse chronological order

#### Scenario: Entry displays reading time and TOC
- **WHEN** a research entry with headings is rendered
- **THEN** the page shows an estimated reading time and a table of contents

### Requirement: Cross-linking between sections
Content SHALL link between Blog and Research Journal using Hugo's `ref` or `relref` shortcodes so that broken targets fail the build.

#### Scenario: Valid cross-link resolves
- **WHEN** the site is built and an entry contains `{{< ref "/blog/example-post" >}}`
- **THEN** the rendered link points to the correct `/site/blog/example-post/` URL

#### Scenario: Broken cross-link fails the build
- **WHEN** a `ref` shortcode targets a non-existent path
- **THEN** `hugo --minify` exits non-zero with an ERROR line

### Requirement: Shared taxonomy for Blog and Research
The site SHALL use a single site-wide `tags` and `categories` taxonomy applied to both Blog posts and Research Journal entries.

#### Scenario: A shared tag aggregates cross-section content
- **GIVEN** a tag `complexity` applied to both a blog post and a research entry
- **WHEN** a user visits `/tags/complexity/`
- **THEN** the page lists both the blog post and the research entry

### Requirement: CV page
The site SHALL provide `content/cv/_index.md` as a single-page layout with placeholder sections for Education, Experience, Skills, and Relevant Coursework.

#### Scenario: CV renders as single page
- **WHEN** a user visits `/cv/`
- **THEN** the page shows all four section headings with placeholder content and no child-page listing

### Requirement: About page
The site SHALL provide `content/about/_index.md` as a single-page layout with placeholder content.

#### Scenario: About renders
- **WHEN** a user visits `/about/`
- **THEN** the page renders with placeholder content

### Requirement: Question Tracker page
The site SHALL provide `content/questions/_index.md` as a living document with an example structure for logging research questions, showing the fields: question, date logged, source post link, and status (open/exploring/resolved).

#### Scenario: Question Tracker renders example entries
- **WHEN** a user visits `/questions/`
- **THEN** the page shows at least one example question row with all four fields populated

### Requirement: Dark/light mode toggle
The site SHALL expose Congo's dark/light mode toggle and default to the `congo` color scheme.

#### Scenario: Toggle is visible
- **WHEN** any page is rendered
- **THEN** the Congo appearance switcher is present in the header

### Requirement: Preserve Hugo 0.160 compatibility shim
The change SHALL NOT remove or modify `layouts/partials/functions/warnings.html`.

#### Scenario: Shim survives the change
- **WHEN** the change branch is merged
- **THEN** `layouts/partials/functions/warnings.html` exists and is byte-identical to its pre-change state

### Requirement: Do not modify vendored theme
The change SHALL NOT modify any file under `themes/congo/`.

#### Scenario: Theme submodule untouched
- **WHEN** `git diff main -- themes/congo` is run after the change
- **THEN** the output is empty (aside from a possible submodule pointer update if deliberately upgraded, which is out of scope)

### Requirement: Placeholder-only content
All content files created by this change SHALL contain clearly marked placeholder text, not final prose.

#### Scenario: Placeholder is recognizable
- **WHEN** any content file created by this change is read
- **THEN** the body contains an explicit placeholder marker such as "TODO:", "[placeholder]", or equivalent

### Requirement: Build passes quality gate
The site SHALL build successfully under the project's backpressure command.

#### Scenario: Production build succeeds
- **WHEN** `hugo --minify` is run at the repo root
- **THEN** the command exits with code 0 and its output contains no lines beginning with `ERROR`

#### Scenario: Dev server starts clean
- **WHEN** `hugo server -D` is run
- **THEN** it starts without emitting any `ERROR` lines
