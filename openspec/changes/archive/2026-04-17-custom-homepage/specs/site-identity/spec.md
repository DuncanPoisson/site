# Delta for Site Identity

## ADDED Requirements

### Requirement: Canonical site title
The site SHALL use `Duncan Xavier Haddock` as its title in `config/_default/hugo.toml` under the `title` key. No config or content file checked into the repository (excluding `themes/congo/` and prior archived changes under `openspec/changes/archive/`) SHALL reference the prior name `Duncan Poisson`.

#### Scenario: Config title is correct
- **WHEN** `config/_default/hugo.toml` is read
- **THEN** the `title` value equals `"Duncan Xavier Haddock"`

#### Scenario: No stale name references
- **WHEN** a case-sensitive grep for `Duncan Poisson` runs across the repository, excluding `themes/`, `public/`, `.git/`, and `openspec/changes/archive/`
- **THEN** no matches are returned

### Requirement: Canonical author name
The site SHALL use `Duncan Xavier Haddock` as the author name in `config/_default/params.toml` under `[author].name`. Pages that render `{{ .Site.Params.author.name }}` or `{{ .Params.author.name }}` SHALL display this value.

#### Scenario: Author param is correct
- **WHEN** `config/_default/params.toml` is read
- **THEN** the `[author].name` value equals `"Duncan Xavier Haddock"`

### Requirement: Header site title color
The header/nav rendering of the site title SHALL be displayed in the primary green color (`#429347`) defined by the active color scheme.

#### Scenario: Title renders green in the built site
- **WHEN** `hugo --minify` has built the site and the root page is viewed in a browser
- **THEN** the computed CSS `color` of the header's site-title element resolves to the primary green (`#429347` or the CSS variable that resolves to it)

#### Scenario: Override lives outside the theme
- **WHEN** inspecting the source of the color override
- **THEN** the rule is located in `assets/css/custom.css` (or another non-theme asset/partial), and no file under `themes/congo/` has been modified
