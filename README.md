# ⚡ CYBERFICTION — Enter the Virtual World

<p align="center">
  <strong>A cinematic, scroll-driven web experience exploring the metaverse, digital worlds, and community through motion.</strong>
</p>

<p align="center">
  <img src="https://img.shields.io/badge/HTML5-E34F26?style=for-the-badge&logo=html5&logoColor=white" alt="HTML5" />
  <img src="https://img.shields.io/badge/CSS3-1572B6?style=for-the-badge&logo=css3&logoColor=white" alt="CSS3" />
  <img src="https://img.shields.io/badge/JavaScript-F7DF1E?style=for-the-badge&logo=javascript&logoColor=black" alt="JavaScript" />
  <img src="https://img.shields.io/badge/GSAP-88CE02?style=for-the-badge&logo=greensock&logoColor=white" alt="GSAP" />
  <img src="https://img.shields.io/badge/Locomotive_Scroll-111111?style=for-the-badge" alt="Locomotive Scroll" />
</p>

## 🎬 Project Preview

<p align="center">
  <img src="./cyberfiction-demo (1).gif"
       alt="Cyberfiction project preview"
       width="100%">
</p>

---

## 🌀 About the Project

**Cyberfiction** is a single-page visual experience built around the idea of a shared virtual world. It combines oversized typography, full-screen sections, smooth scrolling, and a frame-by-frame canvas animation to create a cinematic landing page.

As visitors scroll, a sequence of **300 image frames** plays on a pinned HTML canvas, while the surrounding content introduces the project's ideas about play, community, creativity, and the metaverse.

This project is a front-end exploration of animation-driven storytelling using vanilla web technologies.

## 🎬 Experience Highlights

- 🎞️ **Canvas image-sequence animation** — 300 sequential PNG frames are drawn onto a canvas as the scroll position changes.
- 🖱️ **Smooth scrolling** — Locomotive Scroll provides a smooth scrolling experience inside the main page container.
- 🧭 **Scroll-linked motion** — GSAP and ScrollTrigger coordinate the frame sequence and section pinning.
- 📌 **Pinned full-screen sections** — the experience progresses through distinct, full-height content chapters.
- 🔠 **Animated marquee typography** — the repeated Cyberfiction headline moves continuously across the screen.
- 🌐 **Metaverse-inspired storytelling** — page copy explores community, cooperation, play, and a shared digital playground.
- 📐 **Canvas resizing** — the canvas dimensions update with the browser window, and frames are scaled to fill the view.

## 🧰 Tech Stack

| Technology | Purpose |
|---|---|
| **HTML5** | Page structure, content sections, and canvas element |
| **CSS3** | Full-screen layouts, typography, navigation, and marquee animation |
| **JavaScript (Vanilla)** | Image preloading, canvas rendering, and animation setup |
| **HTML Canvas API** | Draws the current image frame to the screen |
| **GSAP 3** | Coordinates animation timelines and scroll-linked effects |
| **GSAP ScrollTrigger** | Scrubs the frame sequence and pins sections during scrolling |
| **Locomotive Scroll 3** | Smooth scrolling and integration with the custom scroller |

## 🧩 How It Works

### 1. 🖼️ Load the image sequence

The project includes 300 PNG files named `male0001.png` through `male0300.png`. JavaScript creates image objects for the sequence and uses them as the source frames for the canvas.

### 2. 🎨 Render frames on canvas

The `render()` function draws the selected frame onto the canvas. The `scaleImage()` helper calculates a scale that fills the canvas while preserving the image aspect ratio and centering the result.

### 3. 🎚️ Connect animation progress to scrolling

GSAP animates the `imageSeq.frame` value. ScrollTrigger scrubs that value based on scroll progress, so the illustration advances as the visitor scrolls rather than playing only as a conventional timed video.

### 4. 🧲 Coordinate smooth scrolling and pinned sections

Locomotive Scroll manages scrolling on `#main`. `ScrollTrigger.scrollerProxy()` connects that custom scroller to GSAP. The canvas and the following content sections are pinned for defined scroll ranges to create a chapter-by-chapter experience.

### 5. 📰 Add continuous typography

CSS keyframes animate the repeated headline inside `#loop`, creating a continuously moving marquee behind the page's central experience.

## 📂 Project Structure

The current repository stores the source files and image frames in the project root.

```text
Cyberfiction/
├── index.html       # Page content and section structure
├── style.css        # Layout, typography, navigation, marquee
├── script.js        # Smooth scroll, canvas sequence, GSAP triggers
├── male0001.png     # Image sequence (frame 1)
├── male0002.png
├── ...
├── male0300.png     # Image sequence (frame 300)
├── aaaaaaaaaaaa.png # Additional image asset
└── README.md        # Project documentation
```

> 📌 **Asset path note:** The JavaScript currently loads the frame files using root-relative paths such as `./male0001.png`. Keep the images in the repository root unless you also update the paths in `script.js`.

## 🚀 Run Locally

Because this project loads libraries through CDNs and draws images onto a canvas, running it through a local web server is recommended.

### 1. Clone the repository

```bash
git clone https://github.com/Krishnendu1910/Cyberfiction.git
cd Cyberfiction
```

### 2. Start a local server

With Python installed:

```bash
python -m http.server 8000
```

### 3. Open the experience

Visit [http://localhost:8000](http://localhost:8000) in a modern browser and scroll through the page.

> 🌐 **Internet required:** Locomotive Scroll, GSAP, and ScrollTrigger are currently loaded from CDNs.

## ⚙️ Main Files at a Glance

| File | Responsibility |
|---|---|
| `index.html` | Defines the navigation, scrolling container, canvas, and story sections |
| `style.css` | Controls the page layout, full-screen sections, marquee, and text positioning |
| `script.js` | Initializes scrolling, loads image frames, renders the canvas, and configures ScrollTrigger |
| `male0001.png` – `male0300.png` | Sequential frames used for the scroll-driven visual animation |

## 💡 What This Project Demonstrates

- Building immersive effects with vanilla JavaScript.
- Drawing and scaling image sequences using the Canvas API.
- Mapping scroll position to animation progress.
- Coordinating a custom smooth-scrolling library with GSAP ScrollTrigger.
- Pinning sections to create a structured visual narrative.
- Combining motion, typography, and layout into a cohesive landing page.

## 🔧 Ideas for Future Improvements

- [ ] Add a loading indicator while the image sequence is being prepared.
- [ ] Handle failed or not-yet-loaded frames gracefully.
- [ ] Optimize the PNG sequence using WebP/AVIF and measure the visual quality.
- [ ] Add a reduced-motion alternative for visitors who prefer less animation.
- [ ] Provide a fallback experience if JavaScript or an external CDN fails.
- [ ] Add a short animated preview and screenshots to this README.

## 🙌 Credits & Attribution

This repository is a front-end implementation inspired by the Cyberfiction/metaverse visual concept. If you adapt source code, design, or assets from another creator, make sure you review the original license and include any required attribution.

---

<p align="center">
  <strong>🌐 Scroll in. Explore the story. Imagine another digital world.</strong>
</p>

<p align="center">
  <a href="https://github.com/Krishnendu1910">GitHub Profile</a>
  ·
  <a href="https://github.com/Krishnendu1910/Cyberfiction">Repository</a>
</p>
