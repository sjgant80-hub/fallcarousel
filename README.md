# ◊ fallcarousel

> Sovereign single-file LinkedIn carousel builder · prime 367 · v1.0

**Live:** https://sjgant80-hub.github.io/fallcarousel/

A one-file HTML tool that builds LinkedIn document carousels in the format that empirically outperforms text posts by **1.39× reach** and **1.30× engagement** (AuthoredUp 3M+ post study, Feb 2026). LinkedIn Content-Creation topics get a further **1.61× topic reach** on top of that. The carousel you're reading on the live URL was built with this tool. The medium is the marketing.

---

## For the user

You write an outline. The tool renders LinkedIn's native portrait format (1080×1350, 4:5) live in the middle pane. You pick a theme. You hit Export PDF. You upload to LinkedIn as a document post. People swipe through.

That's it. No login. No backend. Your data never leaves your browser.

### Features

- **Outline editor** — title + 8 (free) / 15 (paid) slides, each with eyebrow, headline, 1–5 bullets, optional quote
- **Slide types** — cover, standard, quote-only, CTA
- **Hook templates** — results-first, before/after, contrarian, number-list
- **8 theme presets** — oxblood/brass · navy/orange · mono/yellow · cream/oxblood · forest/gold · rose/wine · ivory/ink · violet/gold · plus full custom bg/fg/accent colour pickers
- **Live preview** at the exact 1080×1350 ratio LinkedIn renders
- **Filmstrip thumbnails** with click-to-jump and ▲▼ reorder
- **Keyboard nav** — ← → through slides
- **Export PDF** — `window.print()` with `@page { size: 1080px 1350px }`; save-as-PDF in browser dialog → LinkedIn accepts as document post
- **Export PNG sequence** — each slide rasterised to a 1080×1350 PNG via Canvas API (no html2canvas dependency)
- **Export standalone HTML deck** — single-file swipeable preview you can ship alone
- **Export JSON** — for backup or cross-device transfer
- **Drafts** — IndexedDB (localStorage fallback) · save/load/delete
- **Copy-paste post body + first comment** templates
- **Works offline from `file://`** — re-openable in 3 years with no internet
- **PWA** — Add to Home Screen on mobile

### Three-minute usage

1. Tweak the example deck that loads on first run, or hit `+ slide` and start fresh
2. Pick a theme swatch · or use the BG/FG/Accent pickers
3. Click `PDF (print)` → browser print dialog → "Save as PDF"
4. Upload to LinkedIn as a document post
5. Paste the copied body text · pin the first-comment hook

### Free vs paid

| | Free (default) | Paid (Konomi-activated) |
|---|---|---|
| Saved drafts | 5 | unlimited |
| Slides per deck | 8 | 15 |
| Export footer | "◊ made with fallcarousel" baked in | clean |
| Brand kit save | — | coming |
| Scheduled queue | — | coming |

Paste a Konomi `KNM-FC-…` key into the licence modal (click the badge top-right) to activate.

---

## For the developer

### Architecture (doctrine)

- **Single HTML file** · 60KB · no build step · no npm
- **Vanilla JS only** · no frameworks · no jQuery
- **Canvas API** for PNG export · `window.print()` + `@media print` for PDF
- **IndexedDB** primary store · `localStorage` fallback baked in
- **Konomi licence shim** · ~50 lines · prime-367 parity check on key body
- **fallmesh hook** · `BroadcastChannel('fall-signal')` · emits `carousel_ready`, `carousel_saved`, `carousel_exported` with `prime: 367`
- **PWA manifest** · inlined as `data:` URL · install-to-home-screen works
- **Works from `file://`** — no fetch-blocking · zero external deps at runtime (Google Fonts is cosmetic; fallbacks resolve)

### Estate position

- **Repo** · `sjgant80-hub/fallcarousel`
- **Pages** · legacy branch `main` path `/` · `.nojekyll` baked
- **Prime** · 367 (LinkedIn-content topic ring)
- **Licence** · MIT
- **Mesh kind** · `carousel_*` events on `fall-signal`

### Print → PDF mechanics

`exportPDF()` builds a `#printArea` div with one `<section class="slide-print">` per slide at 1080×1350, applies the deck's bg/fg/accent inline, then calls `window.print()`. The `@media print` CSS does:

```css
@page { size: 1080px 1350px; margin: 0; }
.slide-print { width: 1080px; height: 1350px; page-break-after: always; }
```

The browser's Save-as-PDF in the print dialog produces a multi-page PDF where every page is exactly LinkedIn's native portrait document size. No jsPDF. No html2canvas. Boring works.

### PNG mechanics

`renderSlideToCanvas(s, idx, total)` draws each slide to an offscreen 1080×1350 canvas using `ctx.fillText` with a tight word-wrap routine. Files download one-at-a-time with a 120ms stagger so the browser doesn't choke.

### Files

| File | Purpose |
|---|---|
| `index.html` | The entire app · 60KB |
| `README.md` | This file |
| `LICENSE` | MIT · Simon Gant |
| `.nojekyll` | Tell GitHub Pages to skip Jekyll |

### Konomi shim · how to activate

Generate a valid key locally — the test is `(sum of charCodes of body) % 367 === 0` where body is everything after `KNM-FC-`. Roll your own gating layer, or wire to a real Ed25519 backend later. This is the lightweight shim.

```js
// Any string after the prefix that satisfies the parity test works
Konomi.activate('KNM-FC-zzz...');
```

### Mesh events

```js
new BroadcastChannel('fall-signal').onmessage = e => console.log(e.data);
// { kind: 'carousel_exported', prime: 367, source: 'fallcarousel', payload: { format:'pdf', slides:7 } }
```

### Verify locally

```bash
# Just open the file
open index.html        # mac
xdg-open index.html    # linux
start index.html       # windows
```

Or:

```bash
python3 -m http.server 8000
# → http://localhost:8000
```

---

## Why carousels

The format is empirically a reach multiplier on LinkedIn — verified across a 3M-post sample (AuthoredUp, Feb 2026):

- **1.39×** reach vs text-only posts
- **1.30×** engagement vs text-only posts
- **1.61×** further reach lift when the topic is "Content Creation"

That maths is why this tool exists, and why the homepage carousel is itself a fallcarousel export.

---

◊ part of the [AI Native Solutions](https://www.ai-nativesolutions.com) estate · prime 367 · MIT · Simon Gant
