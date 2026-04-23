# blog-layout Specification

## Purpose
TBD - update on next archive. The blog landing page and individual blog post layouts.
## Requirements
### Requirement: Horizontal image+text blog list
The Blog landing page (`content/blog/_index.md`) SHALL display each post as a horizontal row with an optional thumbnail image on the left and title, date, and description on the right.

#### Scenario: Post with thumbnail
- **WHEN** a blog post has a `thumbnail` field in its frontmatter
- **THEN** the blog list shows the image on the left (~150–200px wide) with title, date, and description to the right

#### Scenario: Post without thumbnail
- **WHEN** a blog post has no `thumbnail` field
- **THEN** the blog list shows just the title, date, and description (no placeholder image)

### Requirement: Mobile responsive stacking
On small viewports, the blog list layout SHALL stack vertically: image on top, text below.

#### Scenario: Mobile layout
- **WHEN** viewing `/blog/` on a viewport < 640px
- **THEN** each entry stacks vertically with image above text

### Requirement: Blog post thumbnail support
Blog post frontmatter SHALL support a `thumbnail` field specifying a path to an image.

#### Scenario: Example post includes thumbnail
- **WHEN** viewing the example blog post's frontmatter
- **THEN** a `thumbnail` field is present with a valid image path

### Requirement: Blog single page renders marginalia
A blog post's single page SHALL render any front-matter `marginalia` entries according to the `marginalia` capability. A post without marginalia SHALL render exactly as it does today.

#### Scenario: Post with marginalia
- **WHEN** a blog post under `content/blog/*.md` has a non-empty `marginalia` front-matter list
- **THEN** the rendered single page contains the configured notes per the `marginalia` capability requirements (desktop rail and mobile inline)

#### Scenario: Post without marginalia
- **WHEN** a blog post has no `marginalia` front-matter
- **THEN** the rendered single page is byte-identical (modulo whitespace) to the same post built before this change

