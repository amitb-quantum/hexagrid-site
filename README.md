# HexaGrid site

New, GitHub-first website for HexaGrid. No build step, no framework.

## What this is

A static site: `index.html` + `styles.css`. Every push to `main` deploys it to Cloudflare Workers through a GitHub Action. This repo is the source of truth for the new site.

Live address: https://hexagrid-site.amitb1.workers.dev

Important: `hexagrid.ai` still points at Squarespace. This site is built and tested on the `*.workers.dev` address first. The domain cutover is a separate, later approval — do not attach `hexagrid.ai` to this project until Amit says so.

## Prerequisites

- A GitHub account with access to this repo
- A Cloudflare account with the `hexagrid-site` Worker
- Two GitHub repository secrets, set under Settings, Secrets and variables, Actions:
  - `CLOUDFLARE_API_TOKEN`: a Cloudflare API token from the "Edit Cloudflare Workers" template, scoped to the Cloudflare account
  - `CLOUDFLARE_ACCOUNT_ID`: the Cloudflare account id for that account
- Nothing else to install locally. No Node, no bundler, unless you want to deploy by hand.

## Edit the site

1. Edit `index.html` for copy, `styles.css` for layout and colour.
2. To add a page: create a new `.html` file in this folder, copy the header/footer from `index.html`, and add a link to it in the `<nav>` block.
3. Keep every number traceable to the live HexaGrid record. Do not invent benchmarks, pilots, or customers. The published limits section must stay in — it is part of the pitch, not a footnote to trim.
4. Push to `main`. The GitHub Action deploys automatically. Do not edit the live Worker by hand and then forget the repo, or the next push will overwrite it.

## Run it locally

Open `index.html` directly in a browser, or serve the folder:

```sh
python3 -m http.server 8000
```

Then visit http://localhost:8000/

## Deploy on Cloudflare Workers

Deployment is a Wrangler assets-only Worker, not a Cloudflare Pages project:

- Worker name: `hexagrid-site`
- Configuration: `wrangler.toml` in the repo root
- Assets directory: `.` (the repo root)
- Files excluded from upload by `.assetsignore`: `wrangler.toml`, `README.md`, `.github/`, `.assetsignore` itself
- Production branch: `main`
- No build command, no Worker script

The GitHub Action in `.github/workflows/deploy.yml` runs on every push to `main`, and can also be run by hand from the Actions tab. It checks out the repo, sets up Node 22, and runs `npx wrangler@4 deploy` with the two secrets above.

To deploy by hand instead, from this folder:

```sh
CLOUDFLARE_API_TOKEN=<token> CLOUDFLARE_ACCOUNT_ID=<account-id> npx wrangler@4 deploy
```

Never commit a token. The token belongs in GitHub secrets, or in a local environment variable only.

After a deploy, verify https://hexagrid-site.amitb1.workers.dev renders before doing anything with `hexagrid.ai`.

## Domain cutover (not done yet)

Only after Amit approves it, separately:

1. Transfer `hexagrid.ai` registration to Cloudflare Registrar if Squarespace is the registrar, or just change DNS if it is registered elsewhere.
2. Add `hexagrid.ai` and `www.hexagrid.ai` as custom domains on the `hexagrid-site` Worker.
3. Verify `hexagrid.ai` shows this site, then cancel the Squarespace site plan.
4. Preserve email MX records if mail uses this domain.

## Troubleshooting

- Page looks unstyled: `styles.css` must sit next to `index.html` in the repo root, and the `<link>` in `index.html` must still point at it.
- A push did not deploy: check the Actions tab for the run log. The usual cause is a missing or under-scoped `CLOUDFLARE_API_TOKEN` secret, or a missing `CLOUDFLARE_ACCOUNT_ID` secret. The first run after adding the workflow fails until those secrets exist, which is expected.
- `hexagrid.ai` still shows the old site: that is expected until the cutover above. The `*.workers.dev` address is the new site in the meantime.
