# Delta for site-identity

## MODIFIED Requirements

### Requirement: Header site title color
The header/nav rendering of the site title SHALL be displayed in the primary green color (`#082620`) defined by the active color scheme.

(Previously: primary green `#429347`.)

#### Scenario: Title renders green in the built site
- **WHEN** `hugo --minify` has built the site and the root page is viewed in a browser
- **THEN** the computed CSS `color` of the header's site-title element resolves to the primary green (`#082620` or the CSS variable that resolves to it)

#### Scenario: Override lives outside the theme
- **WHEN** inspecting the source of the color override
- **THEN** the rule is located in `assets/css/custom.css` (or another non-theme asset/partial), and no file under `themes/congo/` has been modified
