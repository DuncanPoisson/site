## ADDED Requirements

### Requirement: Burnt orange horizontal dividers
All `<hr>` elements on the site SHALL render as solid burnt orange (#C74D20) lines, approximately 2–3px thick, with appropriate vertical margin.

#### Scenario: Markdown horizontal rule
- **WHEN** a content page includes a Markdown `---` horizontal rule
- **THEN** the rendered `<hr>` displays as a solid burnt orange line

### Requirement: Divider styling via custom.css
The divider styling SHALL be defined in `assets/css/custom.css`, which Congo loads automatically.

#### Scenario: Custom CSS loaded
- **WHEN** building the site with `assets/css/custom.css` present
- **THEN** the `<hr>` styling applies on all pages without modifying theme files

### Requirement: Dividers usable on homepage
The homepage/about page SHALL use dividers between major content sections.

#### Scenario: Homepage sections separated
- **WHEN** viewing the homepage
- **THEN** visual dividers appear between the hero section and the about content
