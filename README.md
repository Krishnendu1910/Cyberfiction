# CYBERFICTION — Immersive Project Overview

> A compact, developer-focused README that explains what the project is, the tech stack, how it works, required assets, file map, and suggestions to improve performance and portability.

---

## 1. Project — Short description

**CYBERFICTION** is a single-page, scroll-driven immersive landing/experience built with vanilla HTML/CSS/JS and two animation/scroll libraries. It uses a pinned `<canvas>` sprite animation (frame-by-frame images) to create a cinematic progression while the rest of the page is driven by smooth scrolling and section pinning. The visual language and copy hint at a metaverse / community play concept.

## 2. Tech stack

* **Vanilla web**: `HTML5`, `CSS3`, `JavaScript` (ES6)
* **Libraries (CDN)**:

  * Locomotive Scroll `v3.5.4` — smooth scrolling and custom scroller container (`#main`).
  * GSAP `3.11.5` — core animation library.
  * GSAP ScrollTrigger `3.11.5` — scroll-driven triggers and pinning.
* **Assets**: A long list of sequential PNG frames (male0001.png → male0300.png) used to render frame-by-frame animation on a `<canvas>`.

## 3. What the project does (high level)

1. Displays a multi-section fullscreen page (`#page`, `#page1`, `#page2`, `#page3`).
2. Uses Locomotive Scroll to make `#main` the scrolling container and to provide smooth, inertia-like scrolling.
3. Creates a sprite animation on a `<canvas>` by preloading ~300 PNG frames and animating them according to scroll position using GSAP + ScrollTrigger.
4. Pins the canvas section while the scroll progresses through an extended `end` value (`600% top`) so the frame animation plays as the user scrolls.
5. Uses ScrollTrigger pins to hold other sections (`#page1`, `#page2`, `#page3`) full-screen while they animate or remain static.

## 4. File-level map / where features live

```
index.html         -> Page structure, links to style.css and script.js, and references CDN libs.
style.css          -> All visual layout, typography, full-screen sections, marquee, nav.
script.js          -> All JS: locomotive + ScrollTrigger setup, canvas animation loader and render pipeline, ScrollTrigger pin configuration.
/assets/frames/    -> male0001.png ... male0300.png  (frame-by-frame images used by canvas)
```

**Which file implements which feature**

* `index.html`

  * Contains the DOM sections used by ScrollTrigger and Locomotive.
  * `<canvas>` element used for the frame animation.
* `style.css`

  * Fullscreen section sizing (`height:100vh`) and layout positions.
  * The marquee loop (#loop) animation and nav styling.
* `script.js`

  * `locomotive()` function: initializes LocomotiveScroll and connects ScrollTrigger via `scrollerProxy`.
  * `files(index)`: returns the list of relative asset paths for frames.
  * Preloads images into `images[]` and sets up a GSAP tween on `imageSeq.frame`.
  * `render()` & `scaleImage()` implement drawing logic and responsive scaling for the canvas.
  * ScrollTrigger pin creation for the canvas and for `#page1`/`#page2`/`#page3`.

## 5. Important code details & how they work (step-by-step)

### A. Smooth scrolling integration (Locomotive + ScrollTrigger)

* LocomotiveScroll is initialized with `el: document.querySelector('#main')` and `smooth: true`.
* Because scrolling happens on a custom container (`#main`) instead of `window`, `ScrollTrigger.scrollerProxy('#main', { ... })` is used to make GSAP know how to read and set scroll positions. This keeps ScrollTrigger and Locomotive in sync.
* `locoScroll.on('scroll', ScrollTrigger.update)` + `ScrollTrigger.addEventListener('refresh', () => locoScroll.update())` keep both libraries updated.

### B. Frame-by-frame canvas animation

* `files(index)` returns a newline-separated string of frame paths and `files(i)` picks the line at `i` — this is how image URLs are supplied.
* `frameCount = 300` — the script pre-creates `Image()` objects for `i = 0..299` and sets `img.src = files(i)` so images start loading.
* A GSAP tween animates `imageSeq.frame` from `1` to `frameCount-1` with `snap: 'frame'` and `ease: 'none'`. The tween is tied to a `ScrollTrigger` that scrubs (maps scroll position to tween progress), making the animation progress as the user scrolls.
* `render()` draws the currently selected frame image onto the canvas using `scaleImage()` which keeps aspect ratio and centers the image.
* The canvas is pinned by ScrollTrigger while the animation plays (`pin: true`, `end: '600% top'`). The `600%` controls how long the user must scroll to play the full animation — increase/decrease to speed up/slow down the play-through.

### C. Section pinning

* `#page1`, `#page2`, `#page3` each get a pinned ScrollTrigger so they appear as fixed full-screen frames for their `start` → `end` duration. This creates a step-like, chaptered scroll experience.

## 6. Asset requirements

* **Frames**: `male0001.png` → `male0300.png` (as referenced). The code expects them to be reachable via the same relative path used in `files()` (currently `./maleXXXX.png`).
* **Fonts**: CSS uses `font-family: gilroy;` — the font file is not included in the code. Provide `@font-face` or link to a hosted font or fallback fonts.

## 7. Setup & run locally

1. Put `index.html`, `style.css`, `script.js` and the `/assets/frames/` folder (or images in the same folder if you keep `./male0001.png` paths) in a project directory.
2. Serve over a local server (recommended) because some browsers block `file://` image loads for `<canvas>`:

   * Python: `python -m http.server 8000`
   * Node: `npx serve` or `npx http-server`
3. Open `http://localhost:8000` and test.

## 8. Deployment notes

* Use a static host (Netlify, Vercel, GitHub Pages). If images are large, use a CDN or object storage (S3) with proper caching.
* Ensure CORS is configured correctly if frames are hosted on a different domain.

## 9. Performance & optimization recommendations

* **Critical**: Loading 300 full-size PNGs can use a lot of memory and bandwidth. Options:

  * Convert frames to a compressed format (WebP or AVIF) to reduce size.
  * Use a spritesheet or an image sequence delivered as a single large image and draw sub-rects (reduces requests but increases memory usage for a single file).
  * Use a streaming approach / lazy-load frames near the current scroll position instead of preloading everything.
  * Reduce `frameCount` if not all frames are visually necessary.
* **Image decoding**: Use `img.decode()` (promise) to avoid drawImage before decode completes.
* **Request reduction**: Serve frames from HTTP/2 or HTTP/3 so many small requests are cheaper.
* **Use GPU-friendly canvas composition** and avoid frequent full-canvas clears if possible (only redraw the changed region).

## 10. Accessibility & UX suggestions

* Provide a non-JS fallback content block for users with JS disabled.
* Add keyboard navigation and focus management for pinned sections.
* Respect `prefers-reduced-motion`: provide a simpler static image or reduced animation for users who opt out.
* Make text selectable/copyable where appropriate, and ensure color contrast is sufficient.

## 11. Known issues & gotchas found in the provided code

* **Font**: `gilroy` is used but not included — fallback will vary. Add `@font-face` or use system/fallback fonts.
* **files()** relies on `data.split('\n')[index]` — leading/trailing empty lines can create `''` entries. Trim the string before splitting to avoid undefined `img.src`.
* **images[1].onload = render;** — if images load slowly or the index being used doesn't exist, `render` may try to draw `undefined`. Use `images[0]` or ensure `imageSeq.frame` is clamped and that each image has onload handlers.
* **Frame indexing**: The code starts using `imageSeq.frame = 1` and pushes images starting at `i = 0`. Ensure consistent indexing (either 0-based or 1-based) and adjust `files()` accordingly.
* **Hard-coded `600%` end**: This affects scroll distance required. Consider computing based on `frameCount` or viewport size for consistent pacing.

## 12. Quick improvements / snippets

* Trim files list safely:

```js
function files(index){
  const lines = `...`.trim().split(/\r?\n/).map(s => s.trim()).filter(Boolean);
  return lines[index];
}
```

* Respect `prefers-reduced-motion`:

```js
if(window.matchMedia('(prefers-reduced-motion: reduce)').matches){
  // disable scrub animation and show static poster frame
}
```

* Use `img.decode()` when preloading:

```js
img.src = url;
img.decode().then(() => { /* mark ready */ }).catch(()=>{/* fallback */});
```

## 13. Next steps you might ask for

* Convert the frame sequence to a WebP/AVIF sequence and update the loader.
* Build a lazy-loading loader that only keeps N frames ahead/behind in memory.
* Replace Locomotive with native `overflow: auto` + CSS `scroll-snap` if you want to remove the dependency.
* Add a small UI to control animation playback (play/pause/seek).

---

*If you want, I can now (pick one):* add a `README.md` file in the repo with this content (already formatted for copy/paste), produce an optimized loader for streaming frames, or create a smaller demo with 30 frames to test performance. Tell me which next step you want and I'll implement it.
