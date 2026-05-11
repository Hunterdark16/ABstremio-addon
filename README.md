---
title: Archivebate Stremio Addon
emoji: 🔞
colorFrom: orange
colorTo: yellow
sdk: docker
pinned: false
---

# Archivebate Stremio Addon

Lightweight Node.js Stremio add-on scaffold for Archivebate pages, prepared for Hugging Face Spaces Docker deployment.

This version adapts the earlier PimpBunny add-on structure to Archivebate branding, routes, IDs, metadata, and deployment variables. It intentionally returns **external page streams only** and does not extract Archivebate CDN/video URLs. Add a direct playback resolver only for content you own or are authorized to redistribute.

## Files

- `addon.js` — main add-on server.
- `package.json` — Node dependencies.
- `Dockerfile` — Docker deployment for Hugging Face Spaces.
- `.dockerignore` — keeps Docker builds small.

## Hugging Face Spaces setup

Create a Docker Space and upload these files.

Set these variables/secrets:

```env
SPACE_URL=https://YOUR-SPACE-NAME.hf.space
ARCHIVEBATE_BASE_URL=https://archivebate.com
OUTBOUND_PROXY_URL=http://USERNAME:PASSWORD@HOST:PORT
```

`OUTBOUND_PROXY_URL` is optional. If configured, it is used for catalog and metadata page fetches.

Alternative split proxy variables:

```env
OUTBOUND_PROXY_HOST=HOST
OUTBOUND_PROXY_PORT=PORT
OUTBOUND_PROXY_USERNAME=USERNAME
OUTBOUND_PROXY_PASSWORD=PASSWORD
```

Do not set `PORT` on Hugging Face unless you specifically need to override it. The Dockerfile defaults to `7860`.

## Behavior

- Catalog routes are adapted for Archivebate watch pages (`/watch/<id>`).
- Archivebate platform/gender filters are mapped using the site's base64 path style, for example `/platform/<base64-slug>` and `/gender/<base64-slug>`.
- Numeric Stremio search terms or pasted `/watch/<id>` URLs create a direct catalog item for that ID.
- Streams open the matching Archivebate page with `externalUrl`.
- Poster image proxying is off by default; set `PROXY_IMAGES=1` to enable `/imgproxy` when `SPACE_URL` is configured.

## Test locally

```bash
npm install
npm run check
npm start
```

Open:

```text
http://localhost:7860/health
http://localhost:7860/manifest.json
```

## Install in Stremio

After deployment, use:

```text
https://YOUR-SPACE-NAME.hf.space/manifest.json
```
