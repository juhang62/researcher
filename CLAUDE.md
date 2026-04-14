# CLAUDE.md

Personal academic website for Lifeng Han (Professor of Practice, Tulane Math), deployed at https://math.tulane.edu/~lhan1/. Jekyll site, forked from the `researcher` template.

## Local build / serve

Gems are vendored under `vendor/bundle`. Bundler lives in `~/.local/share/gem/ruby/3.0.0/bin` (user install, not on default PATH).

```
export PATH="/home/lf/.local/share/gem/ruby/3.0.0/bin:$PATH"
bundle exec jekyll serve          # http://127.0.0.1:4000/~lhan1/
bundle exec jekyll build          # output in _site/
```

Ruby dev headers (`ruby-dev`) are a system prerequisite for native gem extensions.

## Deployment

Not automated. Build locally, then SSH into the Tulane math server and upload `_site/` contents to `~lhan1/public_html` (or equivalent). Ask before running any deployment command — we haven't scripted it.

## Key files

- [index.md](index.md) — About, Teaching, Research Interest, Publications. Publications section is a Liquid loop over `site.data.publications`; don't hand-edit publication entries here.
- [_data/publications.yml](_data/publications.yml) — **the source of truth for publications**. Newest first.
- [contact.md](contact.md) — Contact page (social links only).
- [_config.yml](_config.yml) — `baseurl: /~lhan1`, nav (About/Contact), no footer.
- [_layouts/default.html](_layouts/default.html) — single layout.
- [_sass/_style.scss](_sass/_style.scss) — site styles. Publications-specific styles are in an `ol.publications` block.

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

## What not to touch without asking

- The `researcher` template's structure (`_layouts`, base SCSS). A thorough design overhaul is planned; small ad-hoc tweaks to layout/CSS should be discussed first.
- [myworks.bib](myworks.bib) — BibTeX source used to populate `publications.yml`; keep it in sync when adding papers, but the YAML is what the site renders.
