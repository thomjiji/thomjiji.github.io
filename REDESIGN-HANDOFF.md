# Website Redesign Handoff — Minimal Hugo Theme

Goal: Strip PaperMod down to nothing and rebuild a minimal Hugo theme in the spirit of andreyor.st and blog.benoitj.ca. No JS, no fluff, no features that exist only to exist.

## Reference Sites

### andreyor.st
- Generator: Hugo (custom hand-written theme, not public)
- CSS: Single hand-written `style.css` (~600 lines unminified), includes its own CSS reset
- Fonts: System font stack only —`-apple-system, BlinkMacSystemFont, "Segoe UI", Helvetica, Arial, sans-serif`. Zero external font requests.
- JS: None.
- Layout: `max-width: 800px`, centered, `padding: 0 20px`
- Dark mode: Native `prefers-color-scheme` media query. No toggle button, no JS, no cookies.
- Post list: Title (large, links to post) + date (right-floated, muted) + tags (tiny, gray `#aaa`). Short excerpt below.
- Accent color: Purple `#624195` (light) / `#7a5fa4` (dark)
- RSS: Custom XSLT stylesheet (`rss.xsl`) makes the raw feed human-readable in browser
- HTML structure: Flat and semantic —`<header>`, `<main>`, `<article>`, `<footer>`. No div soup.
- Notable touches: `.foldlist` uses `<details>`/`<summary>` for collapsible lists. Pixel-art image rendering class. YouTube embed helper that avoids JS by using an iframe aspect-ratio wrapper.

### blog.benoitj.ca
- Generator: Hugo 0.146.1
- CSS: [new.css](https://newcss.net/) (`@exampledev/new.css@1.1.2`) via jsDelivr CDN — a classless CSS framework (~5 KB). Styles raw semantic HTML with zero class attributes needed.
- Fonts: Inter (bundled via new.css)
- JS: None.
- Post list: Bare `<ul>` of `<li><a>title</a><br>date</li>`. No excerpts, no tags on index.
- Approach: The most extreme end of minimal — literally just HTML with a classless stylesheet. The entire homepage template is probably 10 lines of Hugo.

## What to Kill When Migrating from PaperMod

PaperMod ships a lot of features that are pure noise for a personal blog. Everything below can be deleted:

| Feature | Why it can go |
|---|---|
| Fuse.js search | A personal blog has an archive page and RSS. Search is for docs sites. |
| Reading time | Patronizing. Readers know how to scroll. |
| Table of contents | Fine for docs, annoying for prose. Kill it or make it opt-in per-post via front matter. |
| Social share buttons | Nobody clicks these. They also load external JS. |
| Cover / hero images | Adds weight, requires maintenance. Text is the content. |
| Breadcrumb nav | Single-level blog. There's nowhere to breadcrumb to. |
| Theme toggle button | Use `prefers-color-scheme`. Let the OS decide. |
| Edit post link | This is your personal site. |
| Related posts | Adds complexity, questionable value. |
| Comments scaffolding | Add it back if you ever actually want it. |
| Pagination chrome | Previous/Next links are enough. No page numbers, no ellipsis. |
| `<meta>` social graph bloat | Keep `og:title`, `og:description`, `og:url`. Drop the rest. |
| Multiple layouts (home/list/single/terms) | Unify. One list template, one post template. |

## Minimal Theme Architecture to Build

```
themes/minimal/
├── layouts/
│   ├── _default/
│   │   ├── baseof.html      # <html>, <head>, <header>, <main>, <footer>
│   │   ├── list.html        # post list (index, /tags/x, /categories/x)
│   │   └── single.html      # individual post
│   └── index.html           # homepage (can just call list.html)
├── static/
│   └── css/
│       └── style.css        # everything, hand-written, ~200-400 lines
└── theme.toml
```

No partials directory needed to start. Add partials only if `baseof.html` gets unwieldy.

### `baseof.html` skeleton

```html
<!DOCTYPE html>
<html lang="{{ .Site.LanguageCode }}">
<head>
  <meta charset="utf-8"/>
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>{{ if .IsHome }}{{ .Site.Title }}{{ else }}{{ .Title }} — {{ .Site.Title }}{{ end }}</title>
  <link rel="stylesheet" href="/css/style.css"/>
  <link rel="alternate" type="application/rss+xml" href="/index.xml" title="{{ .Site.Title }}"/>
  {{ block "head" . }}{{ end }}
</head>
<body>
  <header>
    <a href="/">{{ .Site.Title }}</a>
    <nav>
      <a href="/posts">Posts</a>
      <a href="/about">About</a>
      <a href="/index.xml">RSS</a>
    </nav>
  </header>
  <main>{{ block "main" . }}{{ end }}</main>
  <footer>
    <p>© {{ .Site.Params.author }} {{ now.Year }}</p>
  </footer>
</body>
</html>
```

### CSS foundations (andreyor.st approach)

```css
/* reset */
*, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }

/* base */
body {
  font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Helvetica, Arial, sans-serif;
  font-size: 1rem;
  line-height: 1.6;
  color: #333;
  background: #fdfdfd;
  padding: 0 1.25rem;
}

main { max-width: 740px; margin: 0 auto; }

/* dark mode — no JS needed */
@media (prefers-color-scheme: dark) {
  body { color: #ddd; background: #1e1e1e; }
  a { color: #7a9dcf; }
}
```

## Key Decisions to Make

1. Classless or classed CSS?
   - Classless (benoitj.ca approach): write clean semantic HTML, let CSS target elements directly. Fast to write, less flexible.
   - Custom classes (andreyor.st approach): more control, slightly more HTML ceremony. Easier to extend later.
   - Recommendation: Start classless, add classes only where element selectors aren't specific enough.

2. Post list: excerpts or just titles+dates?
   - andreyor.st: title + date + tags + short auto-excerpt
   - benoitj.ca: title + date only
   - Hugo has `{{ .Summary }}` (auto) and `<!--more-->` (manual cutoff)

3. Tags/categories: keep or drop?
   - If you barely use them, drop. If you post across distinct topics, keep `/tags/` only. Drop categories.

4. Syntax highlighting:
   - Hugo has built-in Chroma highlighting — no JS needed, renders to inline `<span>` or to a separate CSS file
   - Use `markup.highlight.noClasses = false` and generate a highlight CSS file once: `hugo gen chromastyles --style=monokailight > themes/minimal/static/css/syntax.css`

## Migration Steps

1. `hugo new theme minimal` (or create the directory structure manually)
2. Set `theme = "minimal"` in `hugo.toml`
3. Write `baseof.html`, `list.html`, `single.html`— start with the skeletons above
4. Write `style.css` from scratch (andreyor.st's CSS is a good reference —~200 lines covers everything)
5. Delete PaperMod from `themes/`
6. Audit `hugo.toml`— remove all PaperMod-specific `[params]`
7. Audit post front matter — strip `cover`, `showToc`, `showReadingTime`, etc.
8. Test with `hugo server`

## What andreyor.st Does That's Worth Stealing

- RSS with XSLT: The `rss.xsl` trick makes the feed human-readable in a browser. Copy his approach — it's a nice touch and Hugo makes it easy with a custom RSS layout.
- Date on the right: `float: right` on the date in the post list keeps the title left-aligned and scannable while dates are glanceable on the right margin. Clean.
- Tags styled as noise: Tags in `#aaa` (light gray) signal “metadata, not content.” They don't compete with the title.
- `<details>`/`<summary>` for collapsible content: No JS, native HTML, works everywhere.
- Dark mode colors are slightly warm/muted, not pure `#ffffff`/`#000000`: `#dedddc` and `#1e1e1e` are easier on the eyes than stark contrast.
