---
id: sit-il6x
status: open
deps: [sit-f5ks]
links: []
created: 2026-04-15T23:48:47Z
type: task
priority: 1
assignee: Duncan Haddock
parent: sit-g16i
---
# GitHub Actions: build and deploy to Pages

Create .github/workflows/hugo.yml using actions/checkout@v4 (submodules: recursive), peaceiris/actions-hugo@v3 (pinned, extended), actions/configure-pages@v5 (enablement: true), actions/upload-pages-artifact@v3, actions/deploy-pages@v4. Triggers: push to main + workflow_dispatch. Permissions: contents:read, pages:write, id-token:write. Concurrency group 'pages', cancel-in-progress: false. Build step runs 'hugo --minify' and fails on any ERROR line. Update IMPLEMENTATION_PLAN.md with post-merge manual step (Settings -> Pages -> Source = GitHub Actions).

## Acceptance Criteria

Workflow YAML lints clean (actionlint or yaml-lint); action versions pinned; submodules pulled so themes/congo/ is present in CI; hugo --minify succeeds in workflow dry-run (act or committed to branch); IMPLEMENTATION_PLAN.md documents the one-time Pages source toggle.

## Skills

- hugo-congo
- observability
- security
- backpressure

## Design

- File: `.github/workflows/hugo.yml`; also edit `IMPLEMENTATION_PLAN.md`.
- Two jobs: `build` (checkout, setup-hugo, configure-pages, hugo build, upload-pages-artifact) and `deploy` (needs: build, environment: github-pages, deploy-pages). Standard GitHub-recommended Pages pattern (decision #5).
- `actions/checkout@v4` with `submodules: recursive` — mandatory, Congo is a submodule (risk mitigation from design.md).
- `peaceiris/actions-hugo@v3` with `hugo-version` pinned (e.g., `0.160.x` extended) matching local dev.
- `actions/configure-pages@v5` with `enablement: true` (nudges Pages source toggle — still needs manual UI toggle first time).
- Build step: `hugo --minify 2>&1 | tee build.log; grep -q '^ERROR' build.log && exit 1 || true` — fail on any ERROR line.
- Concurrency: `group: pages`, `cancel-in-progress: false` (don't cancel in-flight deploys).
- Permissions declared at workflow level: `contents: read`, `pages: write`, `id-token: write`.
- `IMPLEMENTATION_PLAN.md` addendum: "Post-merge: Settings → Pages → Source = GitHub Actions (one-time)."
- No secrets required (uses GITHUB_TOKEN + OIDC).

