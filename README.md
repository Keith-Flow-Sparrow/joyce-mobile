# Joyce Messages — Webflow animation

A single self-contained script for the looping iMessage animation (Joyce
chatting with patients) embedded on the Joyce marketing site via Webflow.

Served free over **jsDelivr** straight from this public GitHub repo — no CDN
account, no bucket, nothing for any one person to "own". Whoever maintains this
next just needs push access to this repo.

## What's here

- `joyce-messages.js` — the entire animation. Production React baked in
  (~160 KB / 52 KB gzipped), self-mounts, injects its own scoped styles, and
  loads the brand font **and all 5 conversation images** from Joyce's existing
  Webflow CDN. Nothing else to host.

## Embedding in Webflow

**1. An Embed element** where the animation should appear:

```html
<div id="joyce-messages-root"></div>
```

**2. Page (or site) footer custom code** — `Before </body>`:

```html
<script src="https://cdn.jsdelivr.net/gh/Keith-Flow-Sparrow/joyce-mobile@v2/joyce-messages.js" defer></script>
```

That's the whole integration — one tag. No `JOYCE_ASSET_BASE`, no image uploads;
fonts and images already live on the Webflow CDN.

The phone scales to fill its container (capped at native 402px, then centered),
shrinks intact on mobile, is marked decorative for screen readers, and shows a
static conversation to anyone with "reduce motion" turned on.

## Updating the animation

Source lives in the agency project at `webflow-build/src/`. To change the
conversations or design:

1. Edit `src/`, run `npm run build`.
2. Copy `dist/joyce-messages.js` over the one in this repo, commit.
3. Bump the version tag so jsDelivr serves the new file:
   ```bash
   git tag v2 && git push origin v2
   ```
4. Update `@v1` → `@v2` in the Webflow `<script>` URL.

(jsDelivr caches each tag permanently — always pin a version, never use
`@latest` in production.)

### If image/font URLs change

The CDN URLs are hard-coded near the top of `src/imessage-app.jsx`
(`IMAGE_URLS`) and `src/index.jsx` (`fontCss`). If Joyce re-uploads assets in
Webflow and the URLs change, update them there and rebuild.
