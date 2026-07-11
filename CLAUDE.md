# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Overview

This is a static one-page portfolio site for Mosfeq Anik, published via GitHub Pages at `mosfeqanik.github.io`. It is based on a "Tooplate" HTML/CSS/JS template (Bootstrap 4 + jQuery), with no package manager or test suite. `index.html` is assembled from partials via GitHub Pages' built-in Jekyll build (`{% include %}`) — see Development below.

## Development

`index.html` uses Jekyll `{% include %}` tags to pull in each section from `_includes/*.html` (see Structure). This means **opening `index.html` directly in a browser, or serving it with a plain static server (e.g. `python3 -m http.server`), will NOT render the sections** — you'll see the literal `{% include %}` tags, because only Jekyll processes them. Changes to `_includes/*.html`, `css/`, or `js/` still take effect on refresh once served through Jekyll.

To preview locally, run Jekyll:
```
jekyll serve
```
This machine's system Ruby (2.6, at `/usr/bin/ruby`) is too old for modern Jekyll. A newer Ruby was installed via Homebrew (`brew install ruby`) along with Jekyll 4.4.1, at `/opt/homebrew/opt/ruby/bin/ruby` and `/opt/homebrew/lib/ruby/gems/4.0.0/bin/jekyll`. Put those on `PATH` first, e.g.:
```
export PATH="/opt/homebrew/opt/ruby/bin:/opt/homebrew/lib/ruby/gems/4.0.0/bin:$PATH"
jekyll serve
```
If neither is available (fresh machine), install Jekyll the same way before previewing, or just push and check the live site — GitHub Pages runs Jekyll automatically on every push to the default branch, so the includes always render correctly there even without a local Jekyll install.

- `sass/tooplate-style.scss` is the source of `css/tooplate-style.css`. There is no configured Sass compiler in this repo (no package.json) — if you edit the `.scss`, compile it manually (e.g. with the `sass` CLI: `sass sass/tooplate-style.scss css/tooplate-style.css`) and keep both files in sync. If you only need a small style tweak, editing `css/tooplate-style.css` directly and mirroring the change in the `.scss` is acceptable.
- Deployment is implicit: this is the root of a GitHub Pages user site, so anything pushed to the default branch is live immediately at the site URL (after GitHub's Jekyll build completes, usually under a minute).

## Structure

- `index.html` — the page shell: `<head>`, front matter (`---\n---`, required to trigger Jekyll's Liquid processing), the `{% include %}` calls for each section, and the closing `<script>` tags/inline form-validation script.
- `_includes/` — one file per section, each pulled into `index.html` via `{% include %}`: `nav.html`, `about.html` (`#about`), `projects.html` (`#project`), `resume.html` (`#resume`, Experiences + Education), `contact.html` (`#contact`), `footer.html`. Content edits (bio, project list, resume/experience entries, contact info) happen in the relevant include file, not `index.html`.
- `css/` — Bootstrap, Owl Carousel, Unicons, and the compiled `tooplate-style.css`.
- `js/custom.js` — the only hand-written JS; wires up Headroom.js (sticky nav), Owl Carousel, and smooth scrolling. Other `js/*.js` files are third-party libraries, not to be edited.
- `images/` — profile photo and `images/project/` screenshots referenced by the `#project` section.
- `font/` — Unicons icon font files.

## Notes

- Because this is a personal portfolio, "content" changes (bio text, job history, project entries, links) are as common as code changes — check `index.html` first for these.
- Third-party vendor files (`bootstrap.*`, `jquery*`, `owl.carousel.*`, `popper.min.js`, `Headroom.js`, `unicons.css`) should not be modified; update by replacing with a newer vendored copy if needed.
