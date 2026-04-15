# Implementation Plan — Personal Academic Website

**Change:** `site-architecture` ([openspec/changes/site-architecture/](openspec/changes/site-architecture/))
**Status:** Proposed — ready for bootstrap/build
**Owner:** Duncan Poisson

## Goal

Stand up the structural skeleton of a personal academic website at `https://duncanpoisson.github.io/site/` using Hugo + the Congo theme. This change delivers navigation, sections, taxonomy, cross-linking conventions, and a GitHub Actions deploy pipeline. It does **not** deliver final content — every content file is placeholder only.

## Scope

**In scope**
- Four top-level sections: Home, Blog, Research Journal, CV — plus About and Question Tracker.
- A project-indexed Research Journal: `content/research/<project>/<entry>.md`.
- Shared `tags` + `categories` taxonomy across Blog and Research.
- Reading time + table of contents enabled site-wide for article-type content.
- `ref`/`relref` cross-linking convention, demonstrated in the example post and example research entry.
- Dark/light mode toggle (Congo default).
- GitHub Actions workflow that builds and deploys to GitHub Pages on push to `main`.

**Out of scope**
- Real content, final prose, or personal biography.
- Custom layouts, CSS, or theme modifications.
- Comments, analytics, search, newsletter, multi-language.

## Approach

1. **Config first.** Populate `params.toml` and `menus.toml`, wire the taxonomy in `hugo.toml`. Leave `themes/congo/` and `layouts/partials/functions/warnings.html` untouched.
2. **Content skeleton.** Create section `_index.md` files plus one example blog post and one example research project/entry. Cross-link them with `ref` shortcodes so the example itself exercises the convention.
3. **CI/CD.** Add `.github/workflows/hugo.yml` using the official GitHub Pages action trio (`configure-pages` → `upload-pages-artifact` → `deploy-pages`) with `peaceiris/actions-hugo@v3` and `submodules: recursive` checkout.
4. **Validate.** `hugo --minify` passes with no `ERROR` lines; `hugo server -D` starts clean; shared tag pages surface both Blog and Research entries.

## Key Design Decisions

| Decision | Why |
|---|---|
| Research Journal indexed by project, not date | Research output clusters around long-lived projects; date-flat listing would hide continuity. |
| `ref`/`relref` for all cross-links | Build-time validation — broken links fail the build, not production. |
| Shared site-wide taxonomy | Cross-cutting themes should surface in one place regardless of section origin. |
| `peaceiris/actions-hugo` + official Pages actions (not `gh-pages` branch) | Fewer moving parts, native Pages permissions, matches GitHub's current recommended pattern. |
| Congo `page` homepage layout | Cleanest, matches the "uncluttered statement of direction" brief. |

See [`openspec/changes/site-architecture/design.md`](openspec/changes/site-architecture/design.md) for full rationale and alternatives.

## Deliverables

- `config/_default/params.toml`, `menus.toml`, `hugo.toml` populated.
- Content trees: `content/_index.md`, `content/blog/`, `content/research/example-project/`, `content/cv/`, `content/about/`, `content/questions/`.
- One example blog post and one example research entry, cross-linked.
- `.github/workflows/hugo.yml` deploy pipeline.
- Passing quality gate: `hugo --minify` exits 0 with no `ERROR` lines.

## Post-Merge Manual Step

After merging to `main`:

> **GitHub repo → Settings → Pages → Source = "GitHub Actions"** (one-time).

The first push to `main` after this setting is applied will perform the initial deploy.

## References

- [proposal.md](openspec/changes/site-architecture/proposal.md) — why this change
- [design.md](openspec/changes/site-architecture/design.md) — how it's structured
- [specs/site-architecture/spec.md](openspec/changes/site-architecture/specs/site-architecture/spec.md) — site requirements
- [specs/deployment/spec.md](openspec/changes/site-architecture/specs/deployment/spec.md) — CI/CD requirements
- [tasks.md](openspec/changes/site-architecture/tasks.md) — implementation checklist
