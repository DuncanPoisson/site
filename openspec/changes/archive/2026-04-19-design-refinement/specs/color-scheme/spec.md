# Delta for color-scheme

## MODIFIED Requirements

### Requirement: Light mode palette
The system SHALL use the following colors in light mode:
- Page background (`--color-neutral`): warm tan derived from `#ead4bb` (234, 212, 187), mapped to `--color-neutral-50`
- `--color-neutral-50` through `--color-neutral-950`: warm neutral scale derived from the tan background, with the darker end sliding toward the body-text tone `#0e1416` (14, 20, 22)
- Primary scale (`--color-primary-50` through `--color-primary-950`): forest green centered on `#082620` (8, 38, 32) at the 500 level, with lighter tints above and deeper shades below
- Secondary scale (`--color-secondary-50` through `--color-secondary-950`): burnt orange centered on `#C74D20` (199, 77, 32) at the 500 level — unchanged from the prior spec

(Previously: background derived from `#fff6ed`; primary centered on `#429347`.)

#### Scenario: No purple anywhere
- **WHEN** viewing any page in light mode
- **THEN** no violet or fuchsia colors appear — all previously purple UI elements (links, tags, active states, favicon) use forest green (primary) or burnt orange (secondary)

#### Scenario: Primary 500 resolves to new forest green
- **WHEN** a CSS rule reads `rgba(var(--color-primary-500), 1)` in light mode
- **THEN** the computed color is `rgb(8, 38, 32)` (i.e., `#082620`)

#### Scenario: Neutral background resolves to new tan
- **WHEN** a CSS rule reads `rgba(var(--color-neutral-50), 1)` in light mode
- **THEN** the computed color is `rgb(234, 212, 187)` (i.e., `#ead4bb`)

### Requirement: Dark mode palette
The system SHALL provide a dark mode palette that uses a warm-dark background (not pure black or cool gray), retains the forest green primary and burnt orange secondary at comparable saturation, and SHALL be regenerated around the new primary `#082620` such that primary-colored foreground elements in dark mode remain readable against the dark-neutral background.

(Previously: regenerated around `#429347`.)

#### Scenario: Dark mode toggle
- **WHEN** the user switches to dark mode via Congo's appearance switcher
- **THEN** the background changes to a warm dark tone and primary/secondary colors remain forest green and burnt orange
- **THEN** headings, links, and other primary-colored text meet WCAG AA contrast (≥ 4.5:1) against the dark-neutral background

### Requirement: Navigation structure
The system SHALL present five navigation items in this order: About (→ `/`), Blog (→ `/blog/`), Journal (→ `/journal/`), Gallery (→ `/gallery/`), Projects (→ `/projects/`). There SHALL be no separate "Home" nav item. There SHALL be no `CV` nav item; the `/cv/` URL continues to resolve but is not linked from the primary navigation.

(Previously: fourth nav item was `CV` → `/cv/`.)

#### Scenario: Nav links
- **WHEN** viewing the navigation bar on any page
- **THEN** five items appear in order: About, Blog, Journal, Gallery, Projects
- **THEN** clicking "About" navigates to the site root `/`
- **THEN** clicking "Gallery" navigates to `/gallery/`

#### Scenario: CV not in nav
- **WHEN** inspecting the navigation bar
- **THEN** no `CV` item is present

## ADDED Requirements

### Requirement: Body text color
The system SHALL render default body copy (paragraph text, list text, nav labels, dates, descriptions — every text surface not explicitly colored by another rule) in `#0e1416` (14, 20, 22) in light mode. This SHALL be achieved by sliding the `--color-neutral-900` (or the neutral level Congo reads for body copy) toward that value.

#### Scenario: Paragraph text color
- **WHEN** a paragraph on any rendered page is inspected in light mode
- **THEN** its computed `color` resolves to `rgb(14, 20, 22)` (i.e., `#0e1416`) or a variable that evaluates to it

### Requirement: Header/nav band color
The header/nav band (the bar containing the site title and the primary nav) SHALL render with a distinct background `#d1baa3` (209, 186, 163) in light mode — visibly separated from the page background `#ead4bb`. The implementation SHALL live in `assets/css/custom.css` (or another non-theme asset) and SHALL NOT modify any file under `themes/congo/`.

#### Scenario: Band is visually distinct
- **WHEN** the header is inspected in light mode
- **THEN** its computed background-color resolves to `rgb(209, 186, 163)` (i.e., `#d1baa3`)
- **THEN** the page body background below the header resolves to a different color (`#ead4bb`)

#### Scenario: Override outside the theme
- **WHEN** locating the CSS rule that sets the header band color
- **THEN** it resides in `assets/css/custom.css` (or another project-level asset/partial), and no file under `themes/congo/` has been modified

### Requirement: Bubble text color
Text rendered inside the green bubble/card elements on the homepage (`.home-card` and its variants) SHALL be `#EAF9E1` (234, 249, 225).

#### Scenario: Bubble text is light mint
- **WHEN** any `.home-card` bubble on the homepage is inspected
- **THEN** the computed `color` of its title and description text resolves to `rgb(234, 249, 225)` (i.e., `#EAF9E1`)
