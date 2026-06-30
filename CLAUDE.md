# Playable Demos

This repo contains playable demo ads, each as a standalone `.html` file.

## HTML files must be fully self-contained

Every `.html` demo must work as a single file with **no external dependencies** — no
network requests and no references to other files in the repo. They are dropped into ad
environments that may have no access to the `Assets/` folder or the network.

Concretely:

- **Images / SVG / WebP / fonts** — inline as base64 `data:` URIs (e.g.
  `src="data:image/webp;base64,..."`). Do not reference `Assets/...` or sibling files.
  - Downscale large images before inlining to keep file size reasonable (a logo rarely
    needs to be wider than ~2× its largest displayed size).
- **CSS / JS** — keep inline in `<style>` / `<script>` tags. No external stylesheets or
  scripts.
- **Fonts** — do not use Google Fonts `@import` or any CDN font. Use system fonts
  (e.g. `"Avenir Next", -apple-system, BlinkMacSystemFont, sans-serif`), or embed the
  font as a base64 `@font-face` if a specific face is required.

Allowed external URLs (not fetched at load):

- XML namespaces such as `http://www.w3.org/2000/svg`.
- CTA / click-through destination URLs that are meant to open in a browser.

Before finishing a change to an HTML demo, verify there are no stray local-file or CDN
references, e.g.:

```sh
grep -noE '(src|href)="(Assets/|[^"]*\.(svg|webp|png|jpe?g|css|js))"' file.html   # should be empty
grep -noE 'https?://[^"'"'"' )]+' file.html   # only namespaces + CTA links
```
