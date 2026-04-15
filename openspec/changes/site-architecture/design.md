## Context

Hugo 0.160+ with the Congo theme (installed as a git submodule at `themes/congo/`). `baseURL` is already `https://duncanpoisson.github.io/site/`, which means every built URL lives under the `/site/` subpath — a common source of broken internal links if hand-coded. A local override at `layouts/partials/functions/warnings.html` patches a Hugo 0.160 incompatibility in Congo and must remain.

The site will be authored by a single person (Duncan) as a personal academic website. Content volume will grow over time; the skeleton needs to scale without restructure.

## Goals / Non-Goals

**Goals:**
- Deterministic, repeatable Hugo build that deploys to GitHub Pages at `/site/` via GitHub Actions.
- Section structure that supports dated blog posts AND a project-indexed research journal using the same taxonomy vocabulary.
- Cross-linking between Blog and Research Journal via Hugo's `ref`/`relref` shortcodes so link integrity is checked at build time.
- Easy-to-edit CV and About pages as single markdown files.
- Dark/light mode toggle enabled out of the box (Congo feature).

**Non-Goals:**
- Writing real/final content — placeholders only.
- Custom theming, custom layouts, or CSS changes beyond the existing `warnings.html` shim.
- Comments, analytics, search, or newsletter integrations (can be added later).
- Multi-language support.

## Decisions

### 1. Research Journal organizes by project, not by date
**Decision:** `content/research/<project-slug>/_index.md` for each project, with entries as `content/research/<project-slug>/<entry-slug>.md`. Root `content/research/_index.md` lists projects.

**Rationale:** Research output clusters around projects that span months/years. A flat date-ordered journal would bury project continuity. Hugo's default section + list pages render nested `_index.md` files naturally, so no custom layouts are needed.

**Alternatives considered:** Flat `content/research/*.md` with a `project` frontmatter field. Rejected — project landing pages with their own descriptions would require custom taxonomy pages, and the hierarchical layout is closer to Congo's out-of-box behavior.

### 2. Shared taxonomy for Blog and Research
**Decision:** Enable `tags` and `categories` at the site level (Hugo default taxonomies) with no per-section override. Both sections write to the same taxonomy terms.

**Rationale:** Cross-cutting themes (e.g., `complexity`, `urban-systems`) should surface unified tag pages regardless of origin section. Hugo's default taxonomy handling already does this.

### 3. Cross-links use `ref`/`relref`, not raw paths
**Decision:** All internal links between Blog and Research (and elsewhere) use `{{< ref "path" >}}` or `{{< relref "path" >}}` shortcodes. Document this in the example post/entry.

**Rationale:** `ref` validates the target exists at build time, so moves/renames fail the build rather than silently producing 404s. Also resolves the `/site/` subpath correctly.

### 4. Reading time + TOC enabled globally for Blog and Research
**Decision:** Set `showReadingTime = true` and `showTableOfContents = true` in the `[article]` block of `params.toml`, rather than requiring per-post frontmatter.

**Rationale:** Consistent reading experience; individual posts can still opt out by setting frontmatter overrides. Congo supports both global and per-article params.

### 5. GitHub Actions deploy uses `actions/deploy-pages@v4`
**Decision:** Use the official GitHub Pages deployment pair (`actions/configure-pages`, `actions/upload-pages-artifact`, `actions/deploy-pages`) with Hugo extended built via `peaceiris/actions-hugo@v3`. Trigger: push to `main` + `workflow_dispatch`.

**Rationale:** This is the GitHub-recommended pattern for Pages deployments from Actions (vs. gh-pages branch). Fewer moving parts, native Pages permissions, no branch churn. Must fetch submodules (`submodules: recursive`) to pull the Congo theme.

**baseURL handling:** The `baseURL` in `hugo.toml` is already correct for production. The workflow passes `--minify` and relies on the committed config — no per-env override needed.

### 6. Homepage uses Congo's "page" layout
**Decision:** `content/_index.md` with `layout = "page"` (or appropriate Congo homepage layout) and brief placeholder text linking to About.

**Rationale:** Congo ships multiple homepage layouts (`page`, `profile`, `hero`, `custom`, etc.). `page` is the cleanest, closest to the "uncluttered statement of direction" requirement. Easy to swap later without content changes.

## Risks / Trade-offs

- **Risk:** `ref`/`relref` in the example post points to an entry that must exist at build time. → **Mitigation:** Create the research entry first in the tasks ordering, or use draft-safe placeholder targets. Document the convention in a comment.
- **Risk:** GitHub Pages must be manually set to "GitHub Actions" source on first deploy. → **Mitigation:** Call this out in `IMPLEMENTATION_PLAN.md` migration notes; the workflow includes `enablement: true` in `configure-pages` to nudge this.
- **Risk:** Theme submodule not checked out in CI → 404s or build failure. → **Mitigation:** `submodules: recursive` in the checkout step.
- **Risk:** Hugo 0.160 compat shim (`warnings.html`) silently drifts from upstream Congo. → **Mitigation:** Do not modify or remove; add a note in `LESSONS.md` if encountered during build debugging (not in scope for this change).
- **Trade-off:** Shared taxonomy means a tag page mixes Blog and Research entries. This is intentional (cross-cutting themes) but may feel noisy long-term. Revisit if it becomes a problem.

## Migration Plan

1. Merge change branch to `main`.
2. In the GitHub repo UI: **Settings → Pages → Source = "GitHub Actions"** (one-time).
3. First push to `main` triggers the workflow; verify deploy at `https://duncanpoisson.github.io/site/`.
4. Rollback: revert the merge commit; previous Pages deployment stays live until next successful workflow run.

## Open Questions

- Should the Question Tracker eventually become a data-driven page (YAML/TOML data file + template) rather than a hand-edited markdown list? Out of scope for this change; revisit once real questions accumulate.
