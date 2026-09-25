# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Overview

"Gin Notes" — a personal technical notes site built with Jekyll and deployed via GitHub Pages (`ictgtvt.github.io`). Most changes are adding or editing Markdown notes; there are no tests or linters.

## Commands

```sh
bundle install                          # install gems (github-pages ~> 231, webrick)
bundle exec jekyll serve                # local dev server at http://localhost:4000
bundle exec jekyll serve --livereload   # with live reload
bundle exec jekyll build                # build into _site/ (git-ignored)
```

`_config.yml` is not reloaded by `jekyll serve` — restart the server after editing it.

## Architecture

**Content lives in collections, not posts.** `collections_dir: "_collections"` means each topic is a folder `_collections/_<label>/`, declared in `_config.yml` under `collections:` with a display `name`, a `position` (sidebar order), and `output: true`. `_posts/` holds only a couple of dated blog posts, which the home page lists.

**Adding a new topic** requires both:
1. A new entry in `_config.yml` → `collections:` (label, `name`, next `position`, `output: true`).
2. A matching folder `_collections/_<label>/`.

**Note front matter** uses `name`, not `title`:

```yaml
---
layout: post
name: The Pareto principle
---
```

The sidebar in `_layouts/default.html` iterates `site.collections | sort: "position"`, shows only collections that have a `name`, and links each doc by `page.name`. A note without `name` shows up as a blank link.

**Layouts/theme.** The Minima theme is vendored into `_layouts/`, `_includes/`, `_sass/` (the gem is commented out in `Gemfile` and `_config.yml`) and has been reworked around Bootstrap 5.3 loaded from jsDelivr in `_includes/head.html` / `default.html`. `_layouts/post.html` (used by every note) adds a fixed "information might be obsolete" notice above the content. Site styles come from `assets/main.scss` (Roboto font classes + syntax highlighting); it does not import `_sass/minima.scss`, so edits to `_sass/` have no effect unless that import is added.

**Images** go in `assets/images/` (post images under `assets/images/posts/<date>/`).
