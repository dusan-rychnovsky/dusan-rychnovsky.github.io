# Copilot instructions

Personal blog / homepage published with **GitHub Pages** from the repository root
(`dusanrychnovsky.cz`, see `CNAME`). The site is plain static HTML/CSS with no build
step. Pages is served by **default Jekyll** (there is no `.nojekyll` or `_config.yml`),
so anything in a dot-folder such as `.github/` is never published — this file is safe
and will not appear on PROD.

## General conventions

- **Language: Czech.** All user-facing content (titles, prose, meta descriptions,
  `alt` text, breadcrumbs, tags) is written in Czech.
- **Line endings: LF** for every file.
- Do not introduce a build step, framework, or dependency for the site itself. Pages
  are hand-authored static HTML.
- Keep instruction/config files (like this one) under `.github/` so they are never
  published to the live site. Do **not** place `AGENTS.md` / `CLAUDE.md` at the repo
  root — default Jekyll would copy root markdown into the published site.

## The V2 blog

The current blog lives under `blog/v2/`. Shared styling is in
[`blog/v2/static/blog.css`](../blog/v2/static/blog.css). The homepage/listing is
[`blog/v2/index.html`](../blog/v2/index.html).

Content is grouped into top-level sections (currently **Vaření**, **Finance**, **Go**).
Two page archetypes exist, both already represented in the repo — copy the closest
existing page as your starting point:

- **Listing pages** use `<main class="home">` with `.intro` + a `.resources` or
  `.snippets` list (e.g. [`blog/v2/cooking/resources/index.html`](../blog/v2/cooking/resources/index.html)).
- **Prose articles** use `<article>` with `.lead`, headings, `p.aside`, `.book-card`,
  etc. (e.g. [`blog/v2/cooking/recipes/kroupove-misky.html`](../blog/v2/cooking/recipes/kroupove-misky.html)).

Every page starts with the standard `<head>` (charset, viewport, title, `description`,
Open Graph + Twitter meta, link to `static/blog.css`), the `<details class="sidebar">`
navigation, a `.breadcrumbs` nav, and ends with the GoatCounter `<script>` before
`</body>`.

## Creating a new V2 article

When asked to create a new V2 article, complete **all** of the following:

1. **Match existing layout & styling.** The article's structure, classes, and `<head>`
   must correspond to how the other V2 articles work. Reuse the existing CSS classes
   in [`blog/v2/static/blog.css`](../blog/v2/static/blog.css); only add CSS when a
   genuinely new pattern is needed (e.g. a section-specific tag/badge color).

2. **Set up the folder structure** under `blog/v2/<section>/...`, with an `img/`
   subfolder for any article images.

3. **Update the sidemenu in _every_ existing V2 page**, not just the new one. The
   `<details class="sidebar">` block is duplicated across all V2 HTML files and must be
   kept identical (aside from which link has `class="active"`). Discover the full set
   with a search for the sidebar markup rather than assuming a fixed list — at time of
   writing it spans these files:
   - [`blog/v2/index.html`](../blog/v2/index.html)
   - `blog/v2/cooking/resources/index.html`
   - `blog/v2/cooking/recipes/kroupove-misky.html`
   - `blog/v2/cooking/recipes/vlocky-pres-noc.html`
   - `blog/v2/cooking/techniques/jak-varim-kroupy.html`
   - `blog/v2/finance/mmm/index.html`
   - `blog/v2/finance/mmm/smesne-jednoducha-matematika.html`
   - `blog/v2/finance/mmm/klauni-za-volantem.html`
   - `blog/v2/go/resources/index.html`

   New top-level sections are added at the root level of the sidebar (e.g. **Go** sits
   under **Finance**). Mind the relative path depth (`../../` vs none) per file.

4. **Add a snippet to [`blog/v2/index.html`](../blog/v2/index.html).** Snippets are
   listed **newest first**, so a new article goes at the **top** of `.snippets`. Use the
   existing `<article class="snippet">` markup:
   - **Date published = today**, in both the `datetime` attribute and the visible text,
     e.g. `<time datetime="2026-06-09">9.&nbsp;6.&nbsp;2026</time>` (Czech format,
     non-breaking spaces).
   - Thumbnail points at the article's generated image.
   - Tags reuse the section's tag color class (e.g. `tags-cooking`, `tags-finance`,
     `tags-go`); add a new `tags-<section>` rule to the stylesheet for a new section.

5. **Generate the snippet image.** Each article has its own image that must be
   **visually very pleasing** and **clearly reflect what the article is about**. It does
   **not** need to be consistent with other articles' images — be creative per article —
   **unless** the article is a continuation in a series, in which case match that
   series' visual style.
   - Tooling note: this machine has **Node.js** (no Python / ImageMagick). Images so far
     were drawn programmatically with Node + `@napi-rs/canvas`, rendered at a high
     resolution and downscaled (supersampling) for crisp output, then written as a
     `.jpg` next to the article (e.g. `blog/v2/go/resources/index.jpg`). Do the drawing
     in a throwaway temp directory and clean it up afterwards so no generator code or
     `node_modules` lands in the repo — only the final image is committed.

6. **Verify** the result by opening the new article and the homepage in the browser and
   checking the layout, the active sidebar entry, and the snippet/thumbnail render
   correctly.

## Older content

`blog/` also contains the previous (V1) site (e.g. `blog/go/`, `blog/tea/`,
`blog/cooking/`) using Bootstrap. New work targets **V2**; V1 pages are sometimes the
source material that gets migrated into the V2 style.
