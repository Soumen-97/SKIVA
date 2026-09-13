# SKIVA — Your health, organized over time.

A small, installable, fully client-side app for tracking lab reports for you and
your family — side-by-side comparisons across dates, automatic out-of-range
flagging, trend charts, and PDF import. No backend, no accounts: everything is
saved in the visitor's own browser.

## Files in this repo

```
index.html          the entire app (structure, styles, and logic)
manifest.json        makes the app installable (name, icons, colors)
service-worker.js    caches the app so it works offline after first load
icons/
  icon-192.png        app icon (192×192)
  icon-512.png        app icon (512×512)
```

All four files (plus the `icons/` folder) need to stay in the **same relative
layout** relative to each other — `index.html` refers to `manifest.json`,
`service-worker.js`, and `icons/icon-*.png` by relative path.

## Publish it with GitHub Pages

1. Create a new GitHub repository (public, so Pages is free) and add these
   files to it, keeping the folder structure above. Easiest way: download the
   zip, unzip it, and drag the contents straight into your repo (or `git add`
   them from the command line).
2. Commit and push to your default branch (usually `main`).
3. In the repo, go to **Settings → Pages**.
4. Under **Build and deployment → Source**, choose **Deploy from a branch**.
5. Under **Branch**, choose `main` and folder `/ (root)`, then **Save**.
6. GitHub will give you a URL like `https://yourusername.github.io/your-repo/`
   — it usually takes a minute or two to go live after the first save.
7. Open that URL. On Android/desktop Chrome or Edge you should see an install
   icon in the address bar, and the floating **"Install app"** button in the
   bottom-right corner will trigger the same install prompt. On iPhone/iPad
   Safari, there's no automatic prompt (Apple doesn't support it) — tapping
   the button shows the manual steps (Share icon → "Add to Home Screen").

That's it — no build step, no server, no dependencies to install.

## Making changes after it's live

Because the service worker caches files for offline use, returning visitors
won't automatically see your edits unless you bump the cache name. After
changing `index.html` (or anything else), open `service-worker.js` and change:

```js
const CACHE_NAME = 'skiva-cache-v1';
```
to
```js
const CACHE_NAME = 'skiva-cache-v2';
```
(and so on each time you deploy an update). This tells returning browsers to
fetch the new version instead of serving a stale cached copy.

## Swapping the logo

The current logo is a placeholder — a simple gold "L" seal. To replace it:

1. **In-app mark:** open `index.html`, search for `id="logo-mark"`, and swap
   the `<svg>...</svg>` inside it for your own image, e.g.
   `<img src="icons/icon-192.png" alt="logo" style="width:34px;height:34px;">`.
2. **App icons:** replace `icons/icon-192.png` and `icons/icon-512.png` with
   your own square PNGs at those exact sizes (or update the filenames in
   `manifest.json` if you rename them). Keep them as plain, edge-to-edge
   square images — not transparent PNGs with lots of empty margin — since
   Android may crop them into a circle or rounded square.
3. **Favicon:** there's also a small inline SVG favicon at the top of
   `index.html` (`<link rel="icon" ...>`) you can swap the same way.

## How data storage works

There's no database. Each patient and report is saved in the visitor's own
browser (`localStorage`), scoped to whatever URL they're using — so data on
`https://you.github.io/your-repo/` stays separate from anywhere else the file
might be opened. Use the **Export backup** / **Restore backup** buttons in the
sidebar to move data between devices or browsers, or just to keep a safety
copy.

## A note on PDF import

Uploaded PDFs are parsed client-side (via pdf.js, loaded from a CDN) to guess
at test names/values/reference ranges, and the original file is kept so it
can be viewed again later via "view PDF" next to each report date. Automatic
extraction is best-effort — every import shows an editable preview before
anything is saved, since lab report layouts vary and a misread value is easy
to fix by hand right there.
