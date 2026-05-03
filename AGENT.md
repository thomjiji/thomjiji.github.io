# AGENT.md — thomjiji personal site

Quick context for any future session picking this up.

## Rules

- **Commit messages:** conventional commits format — `feat:`, `fix:`, `style:`, `chore:`, `docs:`, etc.

## What this is

A ground-up rebuild of thomjiji's personal website, replacing a PaperMod-based Hugo site (`/home/thom/code/thomjiji.github.io`). The driving motivation: PaperMod ships too many features that exist for their own sake — reading time, share buttons, Fuse.js search, cover images, breadcrumbs, theme toggles. None of those change whether someone reads the writing.

The reference for what this should feel like: **blog.benoitj.ca** — Hugo + new.css, classless, a post list that is literally a `<ul>` with titles and dates. Ruthlessly minimal. The second reference is **andreyor.st** — same philosophy but with hand-written CSS and more polish (see `REDESIGN-INSIGHTS.md` for detailed notes on both).

## Philosophy

- Every element earns its place. If it doesn't serve the reader, it doesn't exist.
- No external requests. Fonts, CSS, everything served from one origin. Exception: KaTeX loaded from CDN on math pages only.
- Dark mode via `prefers-color-scheme` by default. A minimal toggle button (`[ dark ]` / `[ light ]`) exists as a manual override, saved to `localStorage`.
- Semantic HTML. new.css styles elements directly — no class soup in templates.
- The writing is the product. The site is just the container.

## Current state

Remote: `git@github.com:thomjiji/thomjiji.github.io.git`, branch `main`.
Deployed via GitHub Actions to GitHub Pages at `thomjiji.github.io`.
Old site preserved on `start-from-scratch` branch (untouched).

**Stack:**
- Hugo v0.152.2
- new.css (classless framework, ~5 KB, stored locally at `static/css/new.min.css`)
- Syntax highlighting: Chroma, class-based (`noClasses = false`), github style light / doom-one dark via `@media (prefers-color-scheme: dark)` in `static/css/syntax.css`

**Layouts (all hand-written, minimal):**
- `layouts/_default/baseof.html` — base shell: head, header (site title + nav), body block
- `layouts/index.html` — homepage: `<ul>` of all posts, title + date
- `layouts/_default/list.html` — per-tag pages (`/tags/ssh/` etc.), section pages
- `layouts/taxonomy/terms.html` — `/tags/` listing: tags sorted by post count, with count badge
- `layouts/_default/single.html` — individual post: title, date, tag links, content

**Nav:** Home / Tags / About / RSS

**Content:** Migrated from old PaperMod site. Posts live in `content/posts/`, about in `content/about.md`. Some posts are page bundles (directories with `index.md` + images).

**To run:** `hugo server --buildDrafts` from this directory.

## Color palette

Implemented in `static/css/custom.css` — overrides new.css custom properties, never modify `new.min.css`.

**Light theme:** Anthropic Kraft palette. Warm off-white backgrounds, text-color links (underline only), Kraft tan selection highlight.
- `--nc-bg-1: #FBF7F2`, `--nc-bg-2: #F3EDE4`, `--nc-bg-3: #E8DDD0`
- `--nc-lk-1: #1A1A1A` (text color, no color on links)
- `--nc-ac-1: #D4A27F` (Kraft — selection + mark)

**Dark theme:** Full doom-one palette.
- `--nc-bg-1: #282c34`, `--nc-bg-2: #21242b`, `--nc-bg-3: #3f444a`
- `--nc-tx-1/2: #bbc2cf`
- `--nc-lk-1: #51afef`, `--nc-lk-2: #46D9FF`
- `--nc-ac-1: #2257A0` (selection)

## Changes made (2026-05-03 session)

- **KaTeX math rendering:** opt-in per post via `math: true` front matter. Hugo passthrough extension preserves `$...$` / `$$...$$` delimiters. KaTeX loaded from CDN only on math pages.
- **Tag casing fixes:** standardized SSH across all posts (was mixed `ssh`/`SSH`). Added `content/tags/<name>/_index.md` overrides for lowercase tool-name tags (`sshfs`, `btrfs`, `btrbk`, `snapshot`, `rsync`) so Hugo doesn't title-case them. Fixed `colormanagement`/`colorscience` slugs on newer posts.
- **Removed `ShowToc` front matter** from all posts — was a PaperMod remnant, nothing in the layouts reads it.
- **Tags page redesign:** new `layouts/taxonomy/terms.html` — sorted by post count descending, count shown after tag name, no date.
- **Light theme chroma fix:** when OS is dark but site is manually toggled to light, `@media (prefers-color-scheme: dark)` in `syntax.css` was setting `.chroma` base text color to light blue. Fixed by adding `color: #1f2328` to `:root[data-theme="light"] .chroma`.
- **`image/` → `img/` rename:** committed pre-existing renames for three posts, including flattening the daylight post from a page bundle to a flat `.md`.

## TODO — Home page sections

Currently `layouts/index.html` is a plain post list. A few options discussed if this ever needs expanding:

1. **Freeform intro via `content/_index.md`** — write a short bio or intro in Markdown, render it in `index.html` with `{{ .Content }}` above the post list. Zero extra files, easiest to maintain.
2. **Multiple hard-coded blocks in `index.html`** — just add more template blocks (e.g. "Recent" + "Pinned") directly in the layout. Simple, no new content files needed.
3. **Data files (`data/`)** — structured content (projects, links) in YAML/TOML, looped over in the template. Good for things with a fixed shape that change over time.
4. **New content sections (`content/projects/` etc.)** — separate Hugo sections, each with their own list page, pulled into the home page with `where site.RegularPages "Section" "projects"`.

For a personal blog, option 1 or 2 is almost certainly enough.

## Things not to add back

These were explicitly rejected during the rebuild and should not creep back in:
- JS of any kind (except opt-in per-post embeds like vimeo iframes)
- Fuse.js / search
- Reading time
- Table of contents (unless opt-in per post via front matter)
- Social share buttons
- Cover/hero images
- Breadcrumbs
- Theme toggle
- Related posts
- Heavy OG meta tags (keep only og:title, og:description, og:url)

## Files in this directory

- `hugo.toml` — clean config, no PaperMod params
- `layouts/` — all custom, ~4 files total
- `static/css/new.min.css` — new.css framework (do not edit)
- `static/css/custom.css` — color palette + font overrides
- `static/css/syntax.css` — generated by `hugo gen chromastyles`, safe to regenerate
- `content/` — migrated posts + about
- `.github/workflows/hugo.yml` — GitHub Actions deploy to Pages (triggers on `main`)
- `REDESIGN-INSIGHTS.md` — notes from studying andreyor.st and benoitj.ca
- `REDESIGN-HANDOFF.md` — more detailed implementation notes from early planning
