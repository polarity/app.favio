FAVIO
=====
Zero‑dependency, single‑file favicon generator. Drop or paste an image → get a full favicon pack + ready‑to‑paste HTML snippet... all offline. Demo: [https://polarity.productions/favio/](https://polarity.productions/favio/)

## Features
- Drag & drop, paste, or file select an image (PNG / JPG / WEBP / SVG*).  
- Generates a full set of favicon & app icons (PNG + multi‑resolution <code>favicon.ico</code>).  
- Produces a <code>site.webmanifest</code> + minimal mask icon SVG + copyable HTML snippet.  
- Live configurable base path (root <code>/</code> or relative like <code>favicons/</code>).  
- Offline only: no uploads, no tracking, no external network calls.  
- Everything in a single <code>index.html</code> (inline CSS + JS).  
- High‑quality scaling via Canvas (imageSmoothingQuality = high).  
- Accessible keyboard interaction + paste shortcut support.  

<sub>*SVG is rasterized by the browser at its intrinsic size.</sub>

## Generated Files

| File | Purpose |
|------|---------|
| `apple-touch-icon.png` | iOS home screen icon (180×180) |
| `favicon-16x16.png` | Standard small favicon |
| `favicon-32x32.png` | Standard larger favicon |
| `favicon-48x48.png` | Legacy / optional |
| `favicon.ico` | Multi‑size (16, 32, 48) ICO bundle |
| `android-chrome-192x192.png` | PWA icon (192) |
| `android-chrome-512x512.png` | PWA icon (512) |
| `mstile-150x150.png` | Windows tile image |
| `icon-256x256.png`, `icon-384x384.png` | Extra sizes (optional use) |
| `site.webmanifest` | Basic web app manifest |
| `safari-pinned-tab.svg` | Monochrome mask icon (simplistic) |

All files are bundled into a downloadable ZIP. If you specify a base path (e.g. `favicons/`), the ZIP will contain that directory with the files inside.

## Quick Start
1. Clone / download this repo.  
2. Open `index.html` in any modern browser (Chrome, Firefox, Safari, Edge).  
3. Drop or paste an image ≥ 512×512 (square recommended).  
4. Adjust the Base Path if you want the icons to live somewhere other than the site root.  
5. Click “Download ZIP” & copy the HTML snippet into your `<head>`.  
6. Upload the generated files to the matching directory on your server.  

## Base Path Customization
Enter either:  
- Root-based: `/favicons/` (links resolve from the site root)  
- Relative: `favicons/` (links resolve relative to the page embedding them)  

Trailing slash is enforced after you leave the field. While typing, your input isn’t over-normalized. ZIP contents will never begin with a leading `/` (so it unpacks cleanly).

## Example HTML Snippet

```html
<link rel="apple-touch-icon" sizes="180x180" href="/apple-touch-icon.png">
<link rel="icon" type="image/png" sizes="32x32" href="/favicon-32x32.png">
<link rel="icon" type="image/png" sizes="16x16" href="/favicon-16x16.png">
<link rel="manifest" href="/site.webmanifest">
<link rel="mask-icon" href="/safari-pinned-tab.svg" color="#56d364">
<meta name="msapplication-TileColor" content="#0e0f13">
<meta name="theme-color" content="#0e0f13">
```

If you set a base path like `favicons/`, each `href` is prefixed accordingly (e.g. `favicons/favicon-32x32.png`).

## How It Works
- The selected image is loaded into an off-screen `<canvas>` and center‑cropped to square.  
- For each target size, a new canvas is drawn using high-quality smoothing.  
- PNG blobs are collected; an ICO file is assembled manually (directory + PNG chunks).  
- A basic manifest JSON + simple SVG mask are added.  
- A minimal in-browser ZIP builder (Store method, no compression) packages all assets.  
- No external libraries or build steps required.

## Accessibility & UX
- Drop zone is keyboard focusable (Enter / Space triggers file dialog).  
- Live region updates for generation steps via toasts.  
- Copy button uses Clipboard API with graceful failure messaging.  

## Privacy
Everything stays in your browser memory. No network requests are made (inspect the Network tab—empty after load). Your source image never leaves your machine.

## Limitations
- No advanced compression (PNG is whatever the browser encoder outputs).  
- No SVG simplification or mask autotracing.  
- Manifest is intentionally minimal—edit `name` / `short_name` manually.  
- For non-square images we center-crop; no “fit with padding” option (yet).  

## Roadmap (Ideas)
- Optional padding or background fill for non-square images.  
- Extra iOS / legacy Windows meta tags (configurable).  
- Light/dark theme accent color toggle.  
- Progressive favicon (animated generation preview).  
- LocalStorage persistence of last used base path.  
- Service Worker (optional) to demonstrate PWA usage of generated icons.

## Development Notes
Because everything is inline, edits are instant: just refresh the file. If you want to extend sizes, change the `SIZES` / `EXTRA` arrays inside `index.html`.

## Dark Theme CSS Variables

```css
:root {
  --bg: #0e0f13;
  --panel: #151822;
  --text: #e6e6e6;
  --muted: #a0a4b8;
  --accent: #56d364;
  --danger: #ff6b6b;
  --warn: #f7b500;
  --border: #222536;
}
```