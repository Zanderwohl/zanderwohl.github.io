# zanderwohl.github.io

Personal site for Alexander Lowry, built with [Jekyll](https://jekyllrb.com/)
and hosted on GitHub Pages.

## Local development

Install dependencies once:

```bash
bundle install
```

Then serve the site with live reload:

```bash
bundle exec jekyll serve --livereload
```

The site is at <http://localhost:4000>. Edits to pages, posts and Sass rebuild
automatically; edits to `_config.yml` require a restart.

To build the static site into `_site/` without serving:

```bash
bundle exec jekyll build
```

## Layout

```
_config.yml           Site settings
_layouts/             default → page / post
_posts/               Dated posts (the Drinkrig series)
misc/                 Standalone pages (poems, embedded toys)
resume.html           Resume, with schema.org Person microdata
index.html            Home page
assets/css/style.scss Stylesheet (compiled by Jekyll's Sass converter)
assets/img/           Post images
```

### Posts

Posts are HTML with YAML front matter. A post that belongs to a series sets
`series` and `part`; the post layout uses these to build the breadcrumb and the
series list, and `part` (not the date) determines reading order.

```yaml
---
title: 3D Printing Girders
subtitle: Creating girders by 3D printing.
description: Shown in <meta name="description"> and the feed.
series: Drinkrig
part: 2
permalink: /posts/drinkrig-girders/
---
```

### Markup conventions

Pages are written as semantic HTML rather than Markdown, so the structure
carries the meaning:

- `<article>` for the page's main content, `<section aria-labelledby>` for its
  parts, `<header>` for titles and metadata.
- `<figure>` / `<figcaption>` for every image, pull quote, poem and embed —
  captions are never loose `<p>` tags.
- `<time datetime>` for all dates, including resume date ranges.
- `<blockquote>` for quoted text, `<cite>` for work titles.
- The stylesheet targets elements first and classes only where necessary.

## Deployment

The site targets Jekyll 4, which GitHub Pages does not build natively, so
`.github/workflows/jekyll.yml` builds it with GitHub Actions and publishes the
result. In the repository settings, set **Pages → Build and deployment →
Source** to **GitHub Actions**.
