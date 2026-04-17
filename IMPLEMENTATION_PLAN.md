# Implementation Plan — Design Refinement

**Change:** `design-refinement` ([openspec/changes/design-refinement/](openspec/changes/design-refinement/))
**Status:** Proposed — ready for bootstrap/build
**Owner:** Duncan Xavier Haddock
**Branch:** `ralph/design-refinement`

## Goal

Tighten the site's visual identity and expand its navigation around what Duncan actually wants visitors to see. Three things are off about the current build: the palette reads too light, homepage section 2 is a wall of bulleted questions with no visual rhythm, and the CV tab is a placeholder that'd be better spent on a photo Gallery. This change resets the colors, restructures section 2 around two new photos in a two-row matrix, replaces CV with Gallery, and polishes bubble styling (no more default list dots; centered titles; light-mint text on a deep forest-green background).

## Scope

**In scope**
- Palette regen in `assets/css/schemes/duncan.css` — new primary forest green `#082620`, new neutral background `#ead4bb`, body text `#0e1416`, bubble text `#EAF9E1`; secondary burnt orange `#C74D20` unchanged. Dark mode regenerated.
- Header/nav band `#d1baa3` (distinct from page background) added via a new rule in `assets/css/custom.css`.
- Homepage section 2 rewrite (`layouts/index.html` + `content/_index.md`):
  - Heading text → `What is this Website?` (capital W).
  - Two-row matrix: row 1 text-left + `photo1.png` right (2fr/3fr); row 2 `photo2.png` left + text-right (3fr/2fr).
  - Verbatim authored copy for both rows (see spec).
  - Three bubbles (Blog / Journal / Projects) move below the matrix with refreshed subtitles.
- Homepage section 3 (`Who am I?`): center the More-of-Me card, widen to 50–60%, center its text, update href to `https://linktr.ee/duncanpoisson`.
- Bubble styling pass: remove `<li>` markers, center titles, left-align descriptions, apply new background/text colors to all bubbles.
- Navigation swap in `config/_default/menus.toml`: remove CV, insert Gallery at weight 40, Projects bumps to 50. New order: About, Blog, Journal, Gallery, Projects.
- New `gallery` capability:
  - `content/gallery/_index.md` landing page.
  - Example collection as a page bundle under `content/gallery/<slug>/` with at least one placeholder image.
  - `layouts/gallery/list.html` (responsive 3/2/1 card grid).
  - `layouts/gallery/single.html` (responsive photo grid from `.Resources.ByType "image"` + empty-state message).
- Spec updates to `color-scheme`, `homepage-layout`, `site-identity`, and a new `gallery` spec (delta specs written; main specs updated on archive).

**Out of scope**
- Replacing placeholder gallery photos with final imagery.
- Image pipeline work (srcset, responsive resources, optimization) — revisit when gallery is populated.
- Redesigning Blog / Journal / Projects list layouts — only their bubble subtitles on the homepage change.
- Removing or modifying `content/cv/`. `/cv/` continues to resolve; it's just not linked from the nav.
- Any edits inside `themes/congo/` or `layouts/partials/functions/warnings.html`.

## Approach

1. **Palette first.** Regenerate `duncan.css` around `#082620` primary and `#ead4bb` neutral; keep secondary burnt orange untouched. Regenerate dark mode with a brighter primary 500 so text remains legible. Add the header-band rule in `custom.css`. Validate the header selector against the rendered `public/`.
2. **Homepage CSS, then markup, then copy.** Rewrite the inline `<style>` block in `layouts/index.html` for the matrix, the new bubble colors, the centered wide More-of-Me modifier, and the `list-style: none` reset. Then rewrite the section-2 markup (heading text, two matrix rows, bubble re-order). Then shift `content/_index.md` params so the two matrix paragraphs are stored verbatim (or inline them in the layout — simpler if they're not edited often).
3. **Menus.toml swap.** One-line-in, one-line-out; bump Projects weight.
4. **Gallery scaffold.** Content bundle + two layouts; copy the card pattern from `layouts/projects/list.html` so the Gallery landing feels continuous with the rest of the site.
5. **Validate.** `hugo --minify` must exit 0 with no `ERROR` lines. Then `hugo server` for a manual walk-through: desktop + mobile, light + dark, all five nav targets, gallery landing → collection, `/cv/` still resolves.

## Key Design Decisions

| Decision | Why |
|---|---|
| Palette driven through the scheme file, not per-element overrides | Re-deriving the full primary/neutral scale in `duncan.css` means every surface Congo paints (links, tags, buttons, footer) picks up the new tones automatically. Only two surfaces need explicit `custom.css` rules (header title, header band). |
| Header/nav band via `custom.css` selector override, not a theme partial copy | Smallest surface area. If the selector proves brittle against Congo upgrades, fall back to overriding `layouts/partials/header.html`. |
| Section-2 matrix uses CSS grid with `2fr/3fr` + `3fr/2fr` rows | Photos carry more information than the text blocks, so they get the larger column. Grid collapses cleanly to a single column below 768px. |
| Bubble-group markup stays as `<ul>` with `list-style: none` | Semantic (a list of navigational options), cheap to fix the marker issue. |
| `More of Me` bubble reuses `.home-card` and adds `.home-card-wide` | Preserves visual identity with the three main bubbles while unlocking the centered + wider layout. |
| Gallery images are page-bundle resources, not content files | Adding a photo = dropping a file into the collection's directory. No per-image markdown needed. |
| Example collection ships committed | So the `/gallery/` landing isn't empty on first build, and so Duncan has a template to copy. |
| CV content stays, only nav changes | `/cv/` remains live for anyone with the direct link; we don't delete work. |

## Deliverables

- `assets/css/schemes/duncan.css` — palette regen (primary + neutral + dark mode).
- `assets/css/custom.css` — header-title color value updated to `#082620`; new header-band rule.
- `layouts/index.html` — matrix CSS + markup; new bubble styles; wider centered More-of-Me; list-marker reset.
- `content/_index.md` — section-2 body replaced with two verbatim matrix paragraphs; bubble subtitles updated; hero and bio unchanged; More-of-Me link → `https://linktr.ee/duncanpoisson`.
- `config/_default/menus.toml` — CV replaced with Gallery; Projects bumped to weight 50.
- `content/gallery/_index.md` — gallery landing page.
- `content/gallery/<example-slug>/_index.md` + at least one placeholder image — scaffolded example collection.
- `layouts/gallery/list.html` — responsive collection-card grid.
- `layouts/gallery/single.html` — per-collection photo grid with empty-state.
- Clean `hugo --minify` build, zero `ERROR` lines.

## References

- [proposal.md](openspec/changes/design-refinement/proposal.md) — why this change
- [design.md](openspec/changes/design-refinement/design.md) — how it's structured
- [specs/color-scheme/spec.md](openspec/changes/design-refinement/specs/color-scheme/spec.md) — palette, body text, header band, nav structure
- [specs/homepage-layout/spec.md](openspec/changes/design-refinement/specs/homepage-layout/spec.md) — hero color, section-2 matrix, section-3 bubble, list-marker reset
- [specs/site-identity/spec.md](openspec/changes/design-refinement/specs/site-identity/spec.md) — header-title color value update
- [specs/gallery/spec.md](openspec/changes/design-refinement/specs/gallery/spec.md) — gallery capability (new)
- [tasks.md](openspec/changes/design-refinement/tasks.md) — implementation checklist
