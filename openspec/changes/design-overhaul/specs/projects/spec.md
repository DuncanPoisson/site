## ADDED Requirements

### Requirement: Projects section exists
The system SHALL have a Projects section at `content/projects/` serving as a portfolio showcase, accessible at `/projects/`.

#### Scenario: Projects page loads
- **WHEN** navigating to `/projects/`
- **THEN** the projects landing page renders with a list of project entries

### Requirement: Project page structure
Each project SHALL be a single content page (`content/projects/<name>.md`) with frontmatter supporting: `title`, `description`, `status` (completed/ongoing), `tags`, and `links` (list of labeled URLs).

#### Scenario: Example project exists
- **WHEN** building the site
- **THEN** at least one example project page exists with placeholder content, a status field, and tags

### Requirement: Projects card-grid list layout
The projects landing page SHALL display projects in a card/box-grid layout, responsive across desktop (3 per row), tablet (2 per row), and mobile (1 per row).

#### Scenario: Desktop grid
- **WHEN** viewing `/projects/` on a viewport ≥ 1024px
- **THEN** project cards display 3 per row

#### Scenario: Mobile stacking
- **WHEN** viewing `/projects/` on a viewport < 640px
- **THEN** project cards stack vertically

### Requirement: Project card content
Each project card SHALL display the project title, description, and status badge. Cards link to the individual project page.

#### Scenario: Card displays info
- **WHEN** a project exists with title, description, and status "ongoing"
- **THEN** the card shows the title, description, and an "ongoing" status indicator
