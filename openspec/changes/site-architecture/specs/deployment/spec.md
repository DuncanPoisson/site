# Delta for deployment

## ADDED Requirements

### Requirement: GitHub Actions Pages workflow
The repository SHALL contain `.github/workflows/hugo.yml` that builds the Hugo site and deploys it to GitHub Pages on every push to `main` and on manual `workflow_dispatch`.

#### Scenario: Push to main triggers deploy
- **WHEN** a commit is pushed to the `main` branch
- **THEN** the `hugo.yml` workflow runs, builds with `hugo --minify`, uploads the `public/` directory as a Pages artifact, and deploys to GitHub Pages

#### Scenario: Manual run works
- **WHEN** a maintainer clicks "Run workflow" in the Actions tab on `main`
- **THEN** the same build-and-deploy pipeline executes

### Requirement: Submodule-aware checkout
The workflow SHALL check out the repository with `submodules: recursive` so the Congo theme submodule is available during build.

#### Scenario: Theme is present at build time
- **WHEN** the workflow's build job runs
- **THEN** `themes/congo/` is populated before Hugo is invoked

### Requirement: Hugo extended at a pinned version
The workflow SHALL install Hugo extended at a pinned version (not `latest`) using `peaceiris/actions-hugo@v3` or an equivalent action.

#### Scenario: Build uses pinned Hugo
- **WHEN** the workflow runs
- **THEN** the Hugo version reported in the build log matches the version pinned in `hugo.yml`

### Requirement: Correct baseURL for subpath deploy
The deployed site SHALL serve from `https://duncanpoisson.github.io/site/` with all internal links resolving under that subpath.

#### Scenario: Internal links resolve under /site/
- **WHEN** the deployed homepage is loaded
- **THEN** every navigation link and ref-shortcode link begins with `/site/` and returns HTTP 200

### Requirement: Pages permissions and concurrency
The workflow SHALL declare the minimum permissions required (`contents: read`, `pages: write`, `id-token: write`) and a `concurrency` group of `pages` with `cancel-in-progress: false` to prevent parallel deploys from clobbering each other.

#### Scenario: Second push queues rather than racing
- **WHEN** two pushes to `main` occur within the same minute
- **THEN** the second workflow run waits for the first to finish before deploying
