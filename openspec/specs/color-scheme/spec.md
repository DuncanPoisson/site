## ADDED Requirements

### Requirement: Custom color scheme file
The system SHALL include a color scheme file at `assets/css/schemes/duncan.css` that defines all Congo CSS custom properties (`--color-neutral-*`, `--color-primary-*`, `--color-secondary-*`) using RGB triplet format.

#### Scenario: Color scheme is active
- **WHEN** `params.toml` has `colorScheme = "duncan"`
- **THEN** the site renders using the custom color palette

### Requirement: Light mode palette
The system SHALL use the following colors in light mode:
- Page background (`--color-neutral`): warm cream derived from `#fff6ed` (255, 246, 237)
- `--color-neutral-50` through `--color-neutral-950`: warm gray scale derived from the cream background
- Primary scale (`--color-primary-50` through `--color-primary-950`): forest green centered on `#429347` (66, 147, 71) at the 500 level
- Secondary scale (`--color-secondary-50` through `--color-secondary-950`): burnt orange centered on `#C74D20` (199, 77, 32) at the 500 level

#### Scenario: No purple anywhere
- **WHEN** viewing any page in light mode
- **THEN** no violet or fuchsia colors appear — all previously purple UI elements (links, tags, active states, favicon) use forest green (primary) or burnt orange (secondary)

### Requirement: Dark mode palette
The system SHALL provide a dark mode palette that uses a warm-dark background (not pure black or cool gray), retains the forest green primary and burnt orange secondary at comparable saturation.

#### Scenario: Dark mode toggle
- **WHEN** the user switches to dark mode via Congo's appearance switcher
- **THEN** the background changes to a warm dark tone and primary/secondary colors remain forest green and burnt orange

### Requirement: Favicon override
The system SHALL include a favicon at `static/favicon.png` with a burnt orange (`#C74D20`) background square, replacing the default Congo purple favicon.

#### Scenario: Favicon renders
- **WHEN** loading any page on the site
- **THEN** the browser tab shows a burnt orange square favicon

### Requirement: Navigation structure
The system SHALL present five navigation items in this order: About (→ `/`), Blog (→ `/blog/`), Journal (→ `/journal/`), CV (→ `/cv/`), Projects (→ `/projects/`). There SHALL be no separate "Home" nav item.

#### Scenario: Nav links
- **WHEN** viewing the navigation bar on any page
- **THEN** five items appear in order: About, Blog, Journal, CV, Projects
- **THEN** clicking "About" navigates to the site root `/`

### Requirement: Homepage serves as about page
The root URL (`/`) SHALL render as a content page combining a hero/header section (name and tagline) with the about content. The separate `content/about/` section SHALL be removed.

#### Scenario: Landing page content
- **WHEN** visiting the root URL
- **THEN** the page displays the author's name, a tagline, and about content below
- **THEN** no separate `/about/` URL exists
