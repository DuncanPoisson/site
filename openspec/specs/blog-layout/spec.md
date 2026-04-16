## ADDED Requirements

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
