# CLAUDE.md

Personal academic website for Lifeng Han (Professor of Practice, Tulane Math), deployed at https://math.tulane.edu/~lhan1/. Jekyll site, forked from the `researcher` template.

## Branches

- `main` — single working branch and the GitHub default. Holds the current redesign and is the source of truth for both local edits and the production build that gets uploaded to the Tulane math server.
- `pre-redesign` (tag, not a branch) — points at commit `42649af`, the last state of the old `gh-pages` branch before the minimalist redesign. Kept as a record so the previous design can be inspected or restored with `git checkout pre-redesign`. The old `gh-pages` branch itself has been deleted both locally and on `origin`.

## Local build / serve

Gems are vendored under `vendor/bundle`. Bundler lives in `~/.local/share/gem/ruby/3.0.0/bin` (user install, not on default PATH).

```
export PATH="/home/lf/.local/share/gem/ruby/3.0.0/bin:$PATH"
bundle exec jekyll serve          # http://127.0.0.1:4000/~lhan1/
bundle exec jekyll build          # output in _site/
```

Ruby dev headers (`ruby-dev`) are a system prerequisite for native gem extensions.

**Gotcha — `_config.yml` is not live-reloaded.** Jekyll's `--watch` rebuilds when `_config.yml` changes, but it uses the in-memory config from startup; the file is only re-read when the server restarts. Any edit to nav, baseurl, title, etc. requires Ctrl-C and a fresh `jekyll serve`. Also: never run two `jekyll serve` instances at once against this repo — they both write to `_site/` and race-condition each other. Verify with `ps aux | grep jekyll` before starting a new one.

## Deployment

Not automated. Build locally, then SSH into the Tulane math server and upload `_site/` contents to `~lhan1/public_html` (or equivalent). Ask before running any deployment command — we haven't scripted it.

## Key files

- [index.md](index.md) — About, Teaching, Research Interest, Publications. Publications section is a Liquid loop over `site.data.publications`; don't hand-edit publication entries here.
- [_data/publications.yml](_data/publications.yml) — **the source of truth for publications**. Newest first.
- [contact.md](contact.md) — Contact page (social links only). Has `title: Contact` in front matter so the `<title>` tag becomes "Contact — Lifeng Han".
- [_config.yml](_config.yml) — `baseurl: /~lhan1`, no footer. `site.nav` is a list of `{name, link}` entries; `link` may be an absolute URL, a section anchor (`/#teaching`), or a page path (`/contact`). Template prepends `site.baseurl` automatically.
- [_layouts/default.html](_layouts/default.html) — single layout. Renders the sticky `<header>`, the conditional `<h1>` (only on home), the hamburger button, the `<nav aria-label="Primary">`, and a small inline script that toggles `aria-expanded` and runs the IntersectionObserver for active-section highlighting.
- [_sass/vars.scss](_sass/vars.scss) — design tokens (color, type stack, measure, gutter).
- [_sass/_style.scss](_sass/_style.scss) — layout, header, navbar, hamburger, focus-visible, mobile dropdown.
- [_sass/typography.scss](_sass/typography.scss) — prose styles inside `.content` (headings, paragraphs, publications list, blockquote).
- [_sass/tables.scss](_sass/tables.scss) — table styles (legacy template, untouched).

## Adding a publication

Add a new entry at the **top** of [_data/publications.yml](_data/publications.yml):

```yaml
- authors:
    - "@Han, L."            # @ prefix = Lifeng, rendered bold
    - "*Student, N."        # * prefix = mentored student, rendered with superscript dagger
    - "Coauthor, A. B."     # no prefix = regular coauthor
  title: "..."
  venue: "..."
  year: 2027
  volume: "41(2)"           # optional
  pages: "35-52"            # optional
  doi: "10.xxxx/yyyy"       # optional; makes the title a clickable DOI link
```

The template in [index.md](index.md) parses the sigils and wraps names appropriately. The `† student mentee` footnote is hardcoded below the list.

## Publications design conventions

- Titles are the clickable DOI link, but styled as plain black text — accent color (red) only appears on hover. This is intentional to avoid a "wall of red" reading experience. The DOI is not rendered as separate text; the title link is the only DOI surface.
- Lifeng's name: bold via `@` sigil.
- Student mentees: superscript dagger via `*` sigil (no underline). One footnote explains the dagger.
- Sort order: manual, newest first in the YAML.

## Site design conventions (redesign branch)

**Tone**: minimalist academic. Single-column reading layout, all-serif typography, generous whitespace, hierarchy through type weight and spacing rather than color or boxes.

**Typography**: Source Serif 4 throughout (loaded from Google Fonts in [_layouts/default.html](_layouts/default.html)). Body size scales with `clamp()` so there are no awkward breakpoint jumps. Don't introduce a sans-serif unless we're rethinking the whole system.

**Color tokens** (in [_sass/vars.scss](_sass/vars.scss)):
- `$accent` (deep red `#C8252C`) — *only* for hover states, the focus-visible outline, and intentional highlights. Never for default link color.
- `$link` (ink-navy `#1F3A5F`) — default hyperlink color. Warms to `$accent` on hover.
- `$ink` (`#1a1a1a`) — body text.
- `$muted` (`#6b6b6b`) — metadata, captions, nav items in their resting state.
- `$rule` (`#d8d4c8`) — hairline rules and decorative borders.
- `$bg` (`#fdfcf8`) — warm off-white page background.

**Measure**: `$measure: 760px` for the reading column. The sticky header uses a wider 1040px container at `min-width: 900px` so the name and nav don't cluster in the middle of wide screens.

**Sticky header**: full-width `<header class="site-header">` with translucent background (`rgba($bg, 0.78)`) and `backdrop-filter: blur(12px)` for a frosted-glass effect over scrolled content. Hairline bottom border. Pinned via `position: sticky`.

**Active-section indicator**: a small inline IntersectionObserver script in [_layouts/default.html](_layouts/default.html) adds `.is-active` to the matching nav link as you scroll. The CSS for `.is-active` is just `color: $ink` — same weight as resting state, only color changes.

**Mobile (≤720px)**: nav collapses into a hamburger button. The button is a real `<button type="button" aria-expanded aria-controls aria-label>` (not a checkbox hack), toggled by inline JS that flips `aria-expanded`. CSS targets `[aria-expanded="true"] ~ .primary-nav` to reveal the dropdown. Picking a section auto-closes the menu.

**Accessibility baseline** (don't regress these):
- `<header>` is a banner landmark; nav is `<nav aria-label="Primary">`.
- Home page renders the name as `<h1>`; other pages render it as a plain `<a>` so each page can have its own h1 candidate.
- `:focus-visible` outline (2px `$accent`, 3px offset) on all interactive elements. `:focus { outline: none }` reset means mouse clicks don't show outlines, only keyboard focus.
- All animation (smooth scroll, burger transition) is gated behind `@media (prefers-reduced-motion: no-preference)`.
- The `/` separator between desktop nav items has `aria-hidden="true"`.
- `<title>` is `{{ page.title }} — {{ site.title }}` when `page.title` is set.

## What not to touch without asking

- [myworks.bib](myworks.bib) — BibTeX source used to populate `publications.yml`; keep it in sync when adding papers, but the YAML is what the site renders.
- Tokens in [_sass/vars.scss](_sass/vars.scss) — they're referenced everywhere. Adding a new token is fine; renaming or repurposing an existing one cascades.
