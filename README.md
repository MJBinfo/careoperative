# The Careoperative — website

Source for [careoperative.us](https://careoperative.us), served by GitHub Pages
from the `main` branch.

## Structure

```
index.html, about.html, careoperative.html,
work.html, practices.html, sites.html      — the six pages
style.css                                  — the one stylesheet, shared by every page
_includes/                                 — header, footer, scripts, and the
                                              pre-paint theme-init snippet, shared
                                              by every page via Jekyll includes
assets/                                    — PDFs and other downloadable files
CNAME                                      — custom domain for GitHub Pages
```

There is no build step you need to run yourself: GitHub Pages builds the site
with Jekyll automatically on every push to `main`. Jekyll's role here is
narrow — it only resolves the `{% include %}` tags below, nothing else about
these pages is templated.

## Adding a page

1. Copy the `<head>` and the two `{% include %}` lines (for the header and
   footer/scripts) from an existing page, e.g. `work.html` — every page
   starts with an empty Jekyll front-matter fence (`---` / `---`), which is
   what turns on include processing for that file.
2. Add a link to the new page in `_includes/site-header.html`, in both the
   desktop `.nav-page-links` list and the mobile `#nav-panel` list — that one
   file is shared by every page, so this is the only place a new nav link
   needs to be added.
3. Pass the new page's name to the header include so its own nav link gets
   highlighted, e.g. `{% include site-header.html active="mypage" %}` — the
   value has to match what's compared in the `{% if include.active == "..." %}`
   checks inside `_includes/site-header.html`.

## Editing the shared header, footer, or theme script

Edit the relevant file under `_includes/` — every page picks up the change on
its own, since none of them keep a local copy anymore.

## Dark mode

Each page loads `_includes/theme-init.html` inline in `<head>`, before
anything paints. It reads `localStorage['carop-dark']`, falls back to
`prefers-color-scheme` if that's unset, and adds a `.dark` or `.light` class
to `<html>` immediately, so there's no flash of the wrong theme. The toggle
button's click handler lives in `_includes/site-scripts.html`.

With JavaScript disabled, dark mode still works via the
`@media (prefers-color-scheme: dark)` block in `style.css` — this is
deliberately kept in addition to (not merged into) the `.dark` class rules,
since it's the only thing driving dark mode for no-JS visitors.

## Local preview

The site is plain HTML/CSS with no JavaScript build step, so for most edits
you can just open the `.html` files directly in a browser or serve the
directory with any static file server. The one thing that *won't* work that
way is the `{% include %}` tags — those only resolve through an actual Jekyll
build:

```
gem install bundler jekyll   # if not already installed
jekyll build && jekyll serve
```
