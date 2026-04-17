## 1. Rename site identity

- [ ] 1.1 Update `config/_default/hugo.toml` → `title = "Duncan Xavier Haddock"`.
- [ ] 1.2 Update `config/_default/params.toml` → `[author].name = "Duncan Xavier Haddock"`.
- [ ] 1.3 Grep the repo (excluding `themes/`, `public/`, `.git/`, `openspec/changes/archive/`) for remaining `Duncan Poisson` occurrences and update them (notably `IMPLEMENTATION_PLAN.md` and `content/_index.md` front matter).
- [ ] 1.4 Add a CSS rule in `assets/css/custom.css` (or equivalent non-theme location) that colors the header/nav site title in the primary green `#429347`; verify the selector against the built HTML.

## 2. Placeholder photo asset

- [ ] 2.1 Create `static/images/duncan.jpg` as a small placeholder image committed to the repo so the `<img>` has a valid target.
- [ ] 2.2 Leave a pointer (in tasks or a commit note) that the user will replace this file with a real ~800×800 JPG.

## 3. Custom homepage layout

- [ ] 3.1 Create `layouts/index.html` with the four-section structure: hero, "What is this website?", "Who am I?", quote.
- [ ] 3.2 Add inline `<style>` block scoped to the homepage: hero grid (photo + text, 768px breakpoint for stacking), card grid (three-up row collapsing to column), card styling (background `#429347`, white text, border-radius 8–12px, padding, hover affordance).
- [ ] 3.3 Insert `<hr>` elements between all four sections (styling comes from existing `custom.css`).
- [ ] 3.4 Render the hero name `Duncan Xavier Haddock` as a heading in primary green.
- [ ] 3.5 Render the three section cards (Blog, Journal, Projects) as block-level `<a>` anchors with titles and subtitles exactly as specified.
- [ ] 3.6 Render the single "More of me" card in section 3 linking to `https://linktr.ee/` (add an HTML comment marking the URL as TODO).
- [ ] 3.7 Render the Angela Davis quote as a centered italic `<blockquote>` with attribution.

## 4. Final homepage copy

- [ ] 4.1 Replace `content/_index.md` with the final copy verbatim (hero paragraphs, research-questions body, bio paragraphs) per the project spec.
- [ ] 4.2 Set front matter: `title = "Duncan Xavier Haddock"`, `description` preserved or updated, and any Congo article flags (`showDate`, `showReadingTime`, `showTableOfContents`, `showBreadcrumbs`) set to `false` defensively.

## 5. Build, validate, visual-check

- [ ] 5.1 Run `hugo --minify` and confirm exit 0 with no `ERROR` lines.
- [ ] 5.2 Run `hugo server` and visually verify the homepage at `/` in light mode, dark mode, desktop width, and mobile width (≤768px): hero stacks, cards collapse, dividers are burnt orange, name is green, quote is centered italic.
- [ ] 5.3 Click each of the four cards and confirm they navigate to `/blog/`, `/journal/`, `/projects/`, and `https://linktr.ee/` respectively.
- [ ] 5.4 Confirm `layouts/partials/functions/warnings.html` remains unchanged and no `themes/congo/` file has been modified.

## 6. Documentation follow-through

- [ ] 6.1 Rewrite `IMPLEMENTATION_PLAN.md` to describe this change (goal, scope, approach, deliverables, decisions) and remove references to "Duncan Poisson" and the archived design-overhaul.
