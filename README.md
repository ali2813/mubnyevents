# mubnyevents

Website for **MUBNY Events & Banquets** — a 5,000 sq. ft. banquet hall at
2010 Clinton St, Buffalo, NY 14206, seating 300+ guests.

Static site, no build step, no dependencies to install. It renders with a small
client-side runtime (`support.js`) that mounts React and compiles the markup
inside `<x-dc>`.

## Project layout

| Path | What it is |
|---|---|
| `index.html` | The site source — page markup in `<x-dc>`, component logic in the `<script type="text/x-dc">` block at the end. Edit this. |
| `support.js` | The `dc-runtime` — mounts React 18 and renders `<x-dc>`. Generated; don't hand-edit. |
| `assets/vendor/` | `react` + `react-dom` (18.3.1 UMD), self-hosted so the page needs no CDN. |
| `assets/fonts/` | Instrument Serif + Plus Jakarta Sans `woff2` subsets, self-hosted, wired up via `@font-face` in `index.html`. |
| `photos/` | Venue photography (hero + gallery). |
| `mubny-logo.png` | Logo — favicon, header, footer. |
| `standalone.html` | One-file build with the runtime, React and fonts inlined as base64 — for emailing or opening from disk without a server. Not used for deployment. Regenerate from `index.html` via Claude Design's "deploy" export. |

## Run locally

Any static file server from the repo root:

```bash
python3 -m http.server 4599
```

Then open <http://localhost:4599/index.html>. (Opening `index.html` straight off
the filesystem won't work — the runtime and photos are fetched over HTTP. Use
`standalone.html` for that, or run a server.)

## Edit

- **Content / layout:** the markup is plain HTML with inline styles inside
  `<x-dc>` in `index.html`. Sections are `#top`, `#events`, `#space`, `#gallery`,
  `#availability`, `#rates`. Layout is fluid via `clamp()` and
  `grid-template-columns: repeat(auto-fit, minmax(…, 1fr))` — no media queries.
- **Gallery photos:** the `photos` array in the `<script type="text/x-dc">`
  block (`src`, `title`, `note`, `alt`). Drop a file in `photos/` and point a
  row at it.
- **Stats / events / amenities:** the `stats`, `events`, `amenities` arrays in
  the same block.
- **Staff dashboard:** opens from the footer link, passcode-gated. Add/remove
  reservations; they save to `localStorage` (key `mch-reservations-v1`) and show
  as unavailable on the public calendar. Default passcode `2010`, set via the
  `staffPasscode` prop in the `data-props` attribute on the script tag.

  > The passcode ships in the page and the data lives in the visitor's browser —
  > it's a convenience gate, not a security boundary or a shared database.

## Deploy

Everything is static and self-contained (no external requests at runtime).

- **GitHub Pages:** Settings → Pages → Source: `main`, `/ (root)`.
- **Netlify / Cloudflare Pages:** connect the repo, empty build command,
  publish directory `/`.

## Contact

(716) 951-6433 · (716) 590-9129 · mubny.events@gmail.com
