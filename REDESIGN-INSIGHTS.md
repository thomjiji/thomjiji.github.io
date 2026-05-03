# Redesign Insights

The goal is to rebuild the personal site as something genuinely minimal — not “minimal-themed” but actually minimal. Two reference sites informed this:

andreyor.st — Hugo, custom hand-written CSS, no JS, system fonts, dark mode via `prefers-color-scheme`. The CSS is fully custom and hand-written, everything including the reset. The design philosophy is: semantic HTML, one stylesheet, nothing loaded from anywhere else.

blog.benoitj.ca — Hugo, uses new.css (a ~5 KB classless framework) from a CDN. The most extreme version of minimal: the post list is literally a `<ul>` with titles and dates. No excerpts, no tags on the index. The whole homepage template is probably 10 lines.

## What actually makes these sites feel right

It's not the lack of features per se — it's that every element earns its place. The contrast with PaperMod is stark: PaperMod has reading time, share buttons, a theme toggle, breadcrumbs, cover images, Fuse.js search, related posts. None of these exist on either reference site, and none are missed.

The specific things worth internalizing:

- No external requests at all. No Google Fonts, no CDN scripts, no analytics pixel. The page loads from one origin.
- Dark mode without a toggle. `prefers-color-scheme` does the job. A toggle button is a UI element whose only job is to compensate for a missing OS setting.
- Tags as metadata, not navigation. andreyor.st renders tags in `#aaa` gray — visually demoted. They're there if you want them, invisible if you don't.
- The RSS feed is a first-class link, not buried in a footer icon. Both sites put it in the nav.
- andreyor.st's XSLT RSS trick — a custom `rss.xsl` makes the raw feed render as a readable page in the browser. Small touch, good one.

## What to shed from PaperMod

PaperMod features worth explicitly killing, not just leaving unconfigured:

- Fuse.js search (loads JS, implies a docs-site mental model)
- Reading time (noise)
- Table of contents (opt-in per post at most)
- Social share buttons (never used, load external resources)
- Cover/hero images (maintenance burden, shifts focus away from writing)
- Breadcrumb navigation (there's nowhere to go — it's a flat blog)
- Theme color toggle (replaced by `prefers-color-scheme`)
- Related posts section
- The heavy `<head>` with social graph meta tags beyond the basics

The point isn't minimalism as aesthetic — it's that each of these things adds weight without changing whether someone reads the writing.

## Open decisions for the next session

These should be resolved before writing any templates:

1. Classless or classed CSS? Classless means styling raw semantic HTML elements — fast, clean, less flexible. Classed means more control at the cost of more ceremony in templates. andreyor.st uses classes; benoitj.ca is essentially classless. Starting classless and adding classes only where needed is a reasonable path.

2. Excerpts on the post list or just title + date? andreyor.st shows a short excerpt; benoitj.ca shows nothing. This is a taste call.

3. Keep tags? Only worth keeping if they'd actually be used for navigation. If posts span genuinely different topics, `/tags/` is useful. If not, drop them.

4. New theme from scratch or gut PaperMod's templates? Starting fresh is cleaner — PaperMod's template structure carries assumptions that have to be fought against. A new Hugo theme is a small directory.
