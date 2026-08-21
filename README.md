# ahlehjelm.se

Personal site — a single static page, no build step, no dependencies.
Everything (styles, markup, the animated agentic workflow) lives in `index.html`.

## Files

| File | Purpose |
| --- | --- |
| `index.html` | The whole site |
| `404.html` | Styled not-found page, served automatically by GitHub Pages |
| `favicon.svg` | Browser tab icon |
| `og-image.png` | Preview card for LinkedIn, Slack, X, iMessage (1200×630) |
| `robots.txt`, `sitemap.xml` | Search engine basics |
| `.nojekyll` | Stops GitHub from running Jekyll over the files |
| `CNAME.example` | Rename to `CNAME` only if using the custom domain — see below |

## Publishing

1. Create a repository on GitHub. If you want the address to be
   `https://<username>.github.io`, name the repo exactly that. Any other
   name gives you `https://<username>.github.io/<repo>/`.
2. Upload the contents of this folder to the root of the repo — the files
   themselves, not the folder around them. Drag-and-drop into the GitHub web
   uploader works; make sure `.nojekyll` comes along (it is a hidden file, so
   on macOS press `Cmd + Shift + .` in Finder to see it).
3. In the repo, open **Settings → Pages**. Under *Build and deployment*, set
   **Source** to `Deploy from a branch`, branch `main`, folder `/ (root)`.
   Save.
4. Wait a minute or two, then reload the Pages settings screen — the live
   address appears at the top.

Via git instead:

```bash
git init
git add .
git commit -m "Personal site"
git branch -M main
git remote add origin https://github.com/<username>/<repo>.git
git push -u origin main
```

## Custom domain (ahlehjelm.se)

Only do this once the site is live on the `github.io` address.

1. Rename `CNAME.example` to `CNAME`. It must contain the bare domain and
   nothing else.
2. At your DNS provider, create four `A` records for the apex domain `@`
   pointing to `185.199.108.153`, `185.199.109.153`, `185.199.110.153`,
   `185.199.111.153`, plus a `CNAME` record for `www` pointing to
   `<username>.github.io`.
3. In **Settings → Pages → Custom domain**, enter `ahlehjelm.se` and save.
   Once the check passes, tick **Enforce HTTPS**.

DNS changes can take anywhere from a few minutes to a day to propagate.

If you are *not* using a custom domain, delete `CNAME.example`, and change the
`https://ahlehjelm.se/` URLs in `index.html` (canonical link, `og:url`,
`og:image`), `robots.txt` and `sitemap.xml` to your real address — otherwise
link previews will point at a domain that isn't yours yet.

## Editing

Open `index.html` in any editor and reload the file in a browser to see
changes. Nothing to install or compile.

The workflow animation is driven by the `steps` array near the bottom of the
file. Each entry sets which nodes light up, which wire carries the signal, and
what the log line says — edit the `log` strings to describe a process you have
actually built, and rename the `Trigger` and `Output` labels in the SVG to
match. Timing is controlled by `STEP` (milliseconds per step) and `PAUSE`
(the gap before the run restarts).

The animation respects `prefers-reduced-motion` and pauses when scrolled out
of view.
