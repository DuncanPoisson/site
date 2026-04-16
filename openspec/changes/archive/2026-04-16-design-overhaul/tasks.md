## 1. Color Scheme & Favicon

- [ ] 1.1 Create `assets/css/schemes/duncan.css` with full light-mode palette: warm cream neutrals, forest green primary scale (50–950), burnt orange secondary scale (50–950)
- [ ] 1.2 Add dark-mode block to `duncan.css` with warm-dark background, adjusted neutral/primary/secondary scales
- [ ] 1.3 Set `colorScheme = "duncan"` in `config/_default/params.toml`
- [ ] 1.4 Create `static/favicon.png` — burnt orange (#C74D20) square replacing the default purple

## 2. Navigation & Homepage

- [ ] 2.1 Update `config/_default/menus.toml`: About (→ `/`, weight 10), Blog (→ `/blog/`, weight 20), Journal (→ `/journal/`, weight 30), CV (→ `/cv/`, weight 40), Projects (→ `/projects/`, weight 50)
- [ ] 2.2 Merge `content/about/_index.md` content into `content/_index.md` with hero section (name + tagline) and divider between sections
- [ ] 2.3 Delete `content/about/` directory

## 3. Journal Section (Rename & Layout)

- [ ] 3.1 Rename `content/research/` to `content/journal/` (preserve project structure)
- [ ] 3.2 Update all `ref` / `relref` shortcodes across content files from `/research/...` to `/journal/...`
- [ ] 3.3 Create `layouts/journal/list.html` with box-grid layout: 3 columns desktop, 2 tablet, 1 mobile; warm cream boxes, burnt orange border, green titles

## 4. Blog List Layout

- [ ] 4.1 Create `layouts/blog/list.html` with horizontal image+text layout: thumbnail left (~180px), title/date/description right; mobile stacks vertically
- [ ] 4.2 Add `thumbnail` field to `content/blog/example-post.md` frontmatter with a sample image

## 5. Projects Section

- [ ] 5.1 Create `content/projects/_index.md` with section title and description
- [ ] 5.2 Create `content/projects/example-project.md` with placeholder content, status, tags, and links fields
- [ ] 5.3 Create `layouts/projects/list.html` with card-grid layout (matching journal grid style) including status badges

## 6. Visual Dividers & Custom CSS

- [ ] 6.1 Create `assets/css/custom.css` with burnt orange `<hr>` styling (solid, 2–3px, #C74D20) and appropriate margins
- [ ] 6.2 Add `---` dividers to `content/_index.md` between hero and about sections

## 7. Validation

- [ ] 7.1 Run `hugo --minify` — must exit 0 with no ERROR lines
- [ ] 7.2 Run `hugo server -D` and visually verify: color scheme, nav order, homepage layout, journal grid, blog list, projects grid, dividers, favicon, dark mode toggle, mobile responsiveness
