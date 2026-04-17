# Implementation Plan — Custom Homepage

**Change:** `custom-homepage` ([openspec/changes/custom-homepage/](openspec/changes/custom-homepage/))
**Status:** Proposed — ready for bootstrap/build
**Owner:** Duncan Xavier Haddock
**Branch:** `about-section`

## Goal

Replace the placeholder homepage with a bespoke, owner-authored landing page that introduces Duncan, frames the site's three working areas (Blog, Journal, Projects) with clickable section cards, and closes with an Angela Davis quote. At the same time, rename the site title and author name to "Duncan Xavier Haddock" and render the header title in the site's primary green.

## Scope

**In scope**
- Site rename everywhere outside `themes/`, `public/`, `.git/`, and `openspec/changes/archive/` — `config/_default/hugo.toml` title, `config/_default/params.toml` author name, `content/_index.md` front matter, `IMPLEMENTATION_PLAN.md`.
- Custom root template at `layouts/index.html` (project-level Hugo override that beats Congo's default home layout).
- Four sections separated by burnt orange `<hr>` dividers (styling already in `assets/css/custom.css` from the `visual-dividers` spec):
  1. **Hero** — photo + intro text, name heading in primary green. Photo-left / text-right on desktop; stacked on mobile.
  2. **What is this website?** — green heading, bulleted research questions, transition sentence, three clickable green cards linking to `/blog/`, `/journal/`, `/projects/`.
  3. **Who am I?** — green heading, bio body, single "More of me" card linking to `https://linktr.ee/` (placeholder URL, TODO).
  4. **Quote** — centered italic blockquote attributed to Angela Davis.
- Final copy per the project spec, written verbatim.
- Placeholder `static/images/duncan.jpg` so the build succeeds before a real photo arrives.
- Header/nav site title recolored to primary green (`#429347`) via a CSS rule in `assets/css/custom.css`.

**Out of scope**
- Real photo (user swaps in later).
- Real Linktree URL (user updates later).
- Any edits inside `themes/congo/`.
- Changes to other layouts (blog / journal / projects list pages).
- Removing `layouts/partials/functions/warnings.html` — it must remain untouched.

## Approach

1. **Rename first.** Update title/author config, sweep remaining references to the previous name in tracked files, add the CSS rule that recolors the header site title.
2. **Drop the placeholder asset.** Commit a small placeholder `static/images/duncan.jpg` so subsequent builds succeed.
3. **Build the custom template.** `layouts/index.html` with inline `<style>` for scoped layout CSS (hero grid, card grid, mobile breakpoint at 768px). Use semantic `<a class="home-card">` anchors for the clickable cards.
4. **Write the final copy.** Replace `content/_index.md` with the authored text verbatim and set the front matter.
5. **Validate.** `hugo --minify` exits 0 with no `ERROR` lines. Visually verify at `hugo server`: light + dark, desktop + mobile, all four card targets.

## Key Design Decisions

| Decision | Why |
|---|---|
| Override at `layouts/index.html`, not `layouts/_default/home.html` | Narrowest override that takes over just the root URL without shadowing Congo's broader home-layout assumptions. |
| Layout-specific CSS in an inline `<style>` block | Matches the precedent set by the design-overhaul change; keeps `custom.css` reserved for global rules. |
| Section cards as single `<a>` anchors styled as blocks | Full-rectangle hit target, keyboard focus by default, no JavaScript. |
| Reuse the existing `<hr>` burnt-orange rule | `visual-dividers` spec is the single source of truth; new layout just emits `<hr>` elements. |
| CSS-only header-title recolor (no theme partial copy) | Avoids forking a Congo partial; falls back to partial override only if the selector proves brittle. |
| Commit a placeholder photo instead of deferring | `<img>` has a valid target immediately; user swaps the file later without touching markup. |

## Deliverables

- `config/_default/hugo.toml` — `title = "Duncan Xavier Haddock"`
- `config/_default/params.toml` — `[author].name = "Duncan Xavier Haddock"`
- `assets/css/custom.css` — header-title green rule appended (dividers preserved)
- `static/images/duncan.jpg` — placeholder photo
- `layouts/index.html` — custom home template with four sections + inline styles
- `content/_index.md` — final copy per spec, new front matter
- `IMPLEMENTATION_PLAN.md` — this document, replacing the archived design-overhaul plan
- Clean `hugo --minify` build, zero ERROR lines

## References

- [proposal.md](openspec/changes/custom-homepage/proposal.md) — why this change
- [design.md](openspec/changes/custom-homepage/design.md) — how it's structured
- [specs/site-identity/spec.md](openspec/changes/custom-homepage/specs/site-identity/spec.md) — name + header-color requirements
- [specs/homepage-layout/spec.md](openspec/changes/custom-homepage/specs/homepage-layout/spec.md) — section-by-section requirements
- [tasks.md](openspec/changes/custom-homepage/tasks.md) — implementation checklist
