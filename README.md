# ahlehjelm.se

My personal site. One page, one file, no build step.

Everything the site needs — layout, styles, the animated agentic workflow, and
the script that drives it — lives in a single 25 KB `index.html`. No framework,
no bundler, no `node_modules`, no dependencies to update. Opening the file in a
browser shows exactly what the live site shows.

**Live:** https://ahlehjelm.se

## Decisions worth explaining

**No build step.** A personal page changes a few times a year. A toolchain
would mean the site stops being editable the moment its dependencies go stale.
Plain HTML and CSS still work in five years without maintenance.

**No third-party requests.** No analytics, no tag manager, no CDN, no
webfonts. The page loads from one domain and sends nothing to anyone else,
which means no cookie banner and no GDPR exposure. Typography uses the system
UI font — San Francisco on Apple devices, Segoe UI on Windows — so nothing is
fetched to render text.

**SVG rather than a video or a GIF** for the workflow animation. It stays sharp
at any size, weighs nothing, and the individual nodes stay addressable from
JavaScript, so the execution states are driven by adding and removing classes
rather than by playing a clip.

**Motion is opt-out and considerate.** The animation honours
`prefers-reduced-motion` — reduced-motion visitors get the finished workflow as
a static diagram — and an `IntersectionObserver` pauses it whenever it is
scrolled out of view, so it costs nothing when nobody is looking at it.

**Accessibility checked, not assumed.** Text contrast passes WCAG AA
throughout (body 16.8:1, muted 8.0:1 against the background). The heading
outline runs `h1 → h2 → h3` with no skipped levels. The workflow is exposed to
screen readers as a single labelled image rather than as a stream of changing
status text, and German terms carry `lang="de"` so they are pronounced
correctly.

## Files

| File | Purpose |
| --- | --- |
| `index.html` | The whole site |
| `404.html` | Styled not-found page, served automatically by GitHub Pages |
| `favicon.svg` | Tab icon |
| `og-image.png` | Link preview card, 1200×630 |
| `robots.txt`, `sitemap.xml` | Search engine basics |
| `.nojekyll` | Stops GitHub running Jekyll over the files |
| `CNAME.example` | Template for the custom domain |

## Editing

Open `index.html` in any editor, save, reload the browser. That is the whole
workflow.

The animation is driven by the `steps` array near the bottom of the file. Each
entry declares which nodes light up immediately (`act`), which light up the
moment a signal reaches them (`arrive`), which wire carries that signal
(`edge`, with `dur` in seconds), and what the execution log says. To describe a
different process, edit the `log` strings and the `<text class="n-label">` node
names in the SVG. `STEP` sets the milliseconds per step and `PAUSE` the gap
before the run repeats; keep each `dur` at or just below `STEP` so a signal
arrives before the next step starts.

One SVG gotcha worth remembering: a CSS `transform` **replaces** an element's
`transform` attribute rather than combining with it. The completion badges are
positioned with the attribute, so any transform animation on them needs a
nested `<g>` — the outer one for position, the inner one for the animation.
