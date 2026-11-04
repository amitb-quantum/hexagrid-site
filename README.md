# HexaGrid site

New, GitHub-first website for HexaGrid. No build step, no framework.

## What this is

A static site: `index.html` + `styles.css`. Every push to `main` deploys it on Cloudflare Pages. This repo is the source of truth for the new site.

Important: `hexagrid.ai` still points at Squarespace. This site is built and tested on a `*.pages.dev` address first. The domain cutover is a separate, later approval — do not attach `hexagrid.ai` to this project until Amit says so.

## Prerequisites

- A GitHub account with access to this repo
- A Cloudflare account with a Pages project connected to this repo
- Nothing else to install. No Node, no bundler.

## Edit the site

1. Edit `index.html` for copy, `styles.css` for layout and colour.
2. To add a page: create a new `.html` file in this folder, copy the header/footer from `index.html`, and add a link to it in the `<nav>` block.
3. Keep every number traceable to the live HexaGrid record. Do not invent benchmarks, pilots, or customers. The published limits section must stay in — it is part of the pitch, not a footnote to trim.

## Run it locally

Open `index.html` directly in a browser, or serve the folder:

```sh
python3 -m http.server 8000
```

Then visit http://localhost:8000/

## Deploy on Cloudflare Pages

Project settings that match this repo:

- Framework preset: None (static HTML)
- Build command: (none)
- Output directory: `.` (the repo root)
- Production branch: `main`

After the first deploy, Cloudflare gives the project a `*.pages.dev` address. Verify that address renders before doing anything with `hexagrid.ai`.

## Domain cutover (not done yet)

Only after Amit approves it, separately:

1. Transfer `hexagrid.ai` registration to Cloudflare Registrar if Squarespace is the registrar, or just change DNS if it is registered elsewhere.
2. Add `hexagrid.ai` and `www.hexagrid.ai` as custom domains in the Pages project.
3. Verify `hexagrid.ai` shows this site, then cancel the Squarespace site plan.
4. Preserve email MX records if mail uses this domain.

## Troubleshooting

- Page looks unstyled: `styles.css` must sit next to `index.html` in the repo root, and the `<link>` in `index.html` must still point at it.
- A push did not deploy: check the Pages project is connected to this repo and to the `main` branch, and check the Pages deployment log.
- `hexagrid.ai` still shows the old site: that is expected until the cutover above. The `*.pages.dev` address is the new site in the meantime.
