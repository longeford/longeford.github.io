# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Overview

Personal academic website (About, CV, Research, Teaching, Blog) built with Jekyll and the remote `minima` theme, hosted from the GitHub user-site repo `longeford/longeford.github.io` (GitHub Pages, served at the domain root https://longeford.github.io/; previously named `Oscar-Lange` and before that `Corporate-Analysis`). It was moved to the root so Google can show "Oscar Lange" as the site name, which it only supports per domain, not per subfolder. Avoid further renames: the address is registered in Google Search Console. Old `/Oscar-Lange/...` URLs are kept alive by `jekyll-redirect-from`: every page and post carries a `redirect_from: /Oscar-Lange<its permalink>` line; keep those, but new pages don't need one. The blog covers corporate finance theory and real analysis. There is no Gemfile, build script, linter, or test suite; GitHub Pages builds the site on push.

## Local preview

No Gemfile is committed. To preview locally you need Ruby + Jekyll with the `minima` gem (or the `github-pages` gem), e.g.:

```
gem install jekyll minima
jekyll serve
```

Output goes to `_site/` (gitignored). Always build internal links/assets with `relative_url`: GitHub Pages injects `baseurl` automatically (empty now, at the domain root), so links keep working if the address changes again.

## Architecture

- `_config.yml` holds all site-level settings the pages read from: `author.name`/`author.email`, contact links (`linkedin_username`, `scholar_url`, `institution_url`, `github_username`), `footer.address`/`footer.disclaimer`, optional `photo` and `banner` (About page shows them only if set), `cv_dropbox_url`, `header_pages` (defines the nav tabs and their order), and the blog `permalink` (`/blog/...`). Optional features are driven by whether a key is set, not by flags.
- Top-level pages: `index.md` (About, `layout: about`, served at `/`), `cv.md`, `research.md`, `teaching.md`, `blog.md` (`layout: home`, which lists `site.posts`). The site title (the owner's name) is the only place the name appears; the About page deliberately has no name heading.
- `_layouts/` overrides minima's layouts of the same name; all layouts inherit from `base.html`. `about.html` is local-only. Local `_includes/head.html`, `header.html` and `footer.html` replace minima's. `header.html` is minima 2.5.1's nav (driven by `header_pages`) with one change: tab labels use `nav_title`, falling back to `title`. This lets `index.md` keep `title: Oscar Lange` (equal to `site.title`, so jekyll-seo-tag renders the homepage title as "Oscar Lange | <tagline>" for name searches) while the tab reads "About".
- Local includes: `contact-icons.html` (inline-SVG icon row under the bio), `figure.html` (image with optional caption for use in page Markdown), `dropbox-pdf.html` (CV embed).
- **CV embed**: `_includes/dropbox-pdf.html` takes a Dropbox shared file link and renders it with PDF.js 3.11.174 (cdnjs) onto `<canvas>` elements, fetching from `dl.dropboxusercontent.com` (which sends `Access-Control-Allow-Origin: *`); the download link uses `dl=1`. Don't switch back to an `<iframe>`: `www.dropbox.com` sends `frame-ancestors 'self' https://*.dropbox.com`, which Firefox enforces on its redirect, and the direct host serves the file as an attachment. Updating the CV means overwriting the same file in Dropbox — the shared link stays the same.
- **Styles**: `assets/main.scss` sets minima's Sass variables (font, colours, `$content-width`) *before* `@import "minima"`, then adds custom rules. It needs its empty front matter to be compiled.
- **`_includes/custom-head.html`** (included inside `<head>` by the local `head.html`) loads the Lato Google Font and MathJax v3. The `MathJax` config object (inline `$...$` / `\(...\)`, display `$$...$$` / `\[...\]`) must stay before the `tex-svg.js` script tag.
- **SEO**: `jekyll-seo-tag` (meta/OpenGraph/JSON-LD from `title`, `description`, `image`, `social`, `webmaster_verifications`) and `jekyll-sitemap` (`/sitemap.xml`, `/robots.txt`). Every page sets its own `description:` in front matter; keep that for new pages and posts. `url` is deliberately unset so GitHub Pages injects the correct address (the owner plans to move to a custom domain).
- `_posts/` — posts named `YYYY-MM-DD-slug.md` with front matter `layout: post`, `title`, and optional `categories: [...]` / `tags: [...]`. These render as chips (`_includes/post-taxonomy.html`) on the post and in the blog list, linking to anchors (`#cat-…`, `#tag-…`) on `blog/topics.md` (`/blog/topics/`), which lists posts per category and tag. The permalink has no `:categories`, so categories don't change post URLs. LaTeX is written directly in Markdown using the delimiters above.

## Repo notes

- GitHub Pages publishes every `.md` file in the repo as a page unless it is listed under `exclude:` in `_config.yml`. Add any new non-site Markdown file (notes, docs) there.

- `main` is the default branch on GitHub; push changes there.
- A previous `_projects` collection and `project` layout were deleted; don't reintroduce references to them.
