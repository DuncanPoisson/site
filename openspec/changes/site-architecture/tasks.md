## 1. Site configuration

- [ ] 1.1 Fill out `config/_default/params.toml` with Congo appearance, author info, `showReadingTime`, `showTableOfContents`, dark/light toggle, and shared-taxonomy settings
- [ ] 1.2 Add `[taxonomies]` (tags, categories) and `[outputs]` (home includes RSS) to `config/_default/hugo.toml`
- [ ] 1.3 Create `config/_default/menus.toml` with Home, Blog, Research Journal, CV, About entries
- [ ] 1.4 Review `config/_default/markup.toml` for TOC settings and add `languages.toml` only if Congo requires it

## 2. Content scaffolding

- [ ] 2.1 Create `content/_index.md` with Congo `page` layout, placeholder statement, link to `/about/`
- [ ] 2.2 Create `content/about/_index.md` (single page, placeholder)
- [ ] 2.3 Create `content/cv/_index.md` with Education / Experience / Skills / Relevant Coursework placeholder sections
- [ ] 2.4 Create `content/questions/_index.md` with example question-tracker table (question, date logged, source post link, status)
- [ ] 2.5 Create `content/blog/_index.md` and one example post (`content/blog/example-post.md`) with full frontmatter (title, date, draft, tags, categories, description) and a body containing at least two headings for TOC
- [ ] 2.6 Create `content/research/_index.md` (lists projects)
- [ ] 2.7 Create `content/research/example-project/_index.md` with project description
- [ ] 2.8 Create `content/research/example-project/example-entry.md` with tags, a code block, an image placeholder, and a `{{< ref >}}` cross-link back to the example blog post
- [ ] 2.9 Add a `{{< ref >}}` cross-link in the example blog post pointing at the example research entry

## 3. Deployment pipeline

- [ ] 3.1 Create `.github/workflows/hugo.yml` using `actions/checkout@v4` with `submodules: recursive`, `peaceiris/actions-hugo@v3` pinned, `actions/configure-pages@v5`, `actions/upload-pages-artifact@v3`, `actions/deploy-pages@v4`
- [ ] 3.2 Set workflow triggers: `push` to `main` and `workflow_dispatch`
- [ ] 3.3 Declare permissions (`contents: read`, `pages: write`, `id-token: write`) and concurrency group `pages` with `cancel-in-progress: false`
- [ ] 3.4 Build step runs `hugo --minify` and fails on any `ERROR` line in output

## 4. Validation

- [ ] 4.1 Confirm `layouts/partials/functions/warnings.html` is unchanged and `themes/congo/` has no diffs
- [ ] 4.2 Run `hugo --minify` locally: exit code 0, no `ERROR` lines
- [ ] 4.3 Run `hugo server -D` locally: starts clean, visit each top-level route and confirm links resolve under `/site/`
- [ ] 4.4 Verify shared `/tags/<term>/` page lists both a blog post and a research entry tagged with that term

## 5. Documentation

- [ ] 5.1 Update `IMPLEMENTATION_PLAN.md` with post-merge manual step (Pages → Source = GitHub Actions)
- [ ] 5.2 Note the `ref`/`relref` cross-linking convention in a short authoring comment inside the example post and example research entry
