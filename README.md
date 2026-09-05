# VELTRON — Built For The Obsessed

An immersive, single-page digital showroom for **VELTRON**, a fictional ultra-luxury performance automotive brand. Built as a cinematic scroll experience around an original open-wheel hypercar rendered in real-time 3D — no stock photography, no licensed vehicle designs, no external CDN dependencies.

> VELTRON is a fictional brand created for this project. The V-01 is an original procedural design and is not modeled after any real manufacturer or vehicle.

---

## What this is

A single HTML file — `index.html` — containing the entire site: markup, styles, and JavaScript, plus every third-party library it depends on, all inlined. There is nothing else to install, build, bundle, or host separately.

```
dist/
└── index.html      ← the whole site. This is the only file you need to deploy.
```

Open it directly in a browser, or upload it to any static host and it works immediately.

## Why fully self-contained

Earlier drafts loaded Three.js, GSAP, ScrollTrigger, and Lenis from public CDNs. In practice, any one of those failing to load (ad blockers, corporate proxies, regional blocks, a flaky CDN) would silently break the entire page, since every interactive feature depends on `gsap` being defined. To remove that class of failure entirely, every dependency is now bundled directly inside `index.html`:

- **Three.js** r160 — 3D rendering
- **GSAP** + **ScrollTrigger** — animation orchestration
- **Lenis** — smooth scrolling

Fonts use the system default stack (`-apple-system, Segoe UI, Roboto, Helvetica Neue, Arial, sans-serif`) rather than a custom webfont, for the same reason: one less thing that can fail to load.

## Deployment

Any static host works. A few common options:

**GitHub Pages**
1. Commit `index.html` to your repository (rename it if your host expects a different entry filename).
2. Enable Pages in the repo settings, pointing at the branch/folder containing it.

**Netlify / Vercel / Cloudflare Pages**
Drag-and-drop the file (or the folder containing it) into the dashboard, or connect the repo — no build command needed, no framework preset required.

**Anywhere else**
Any server that can serve a static `.html` file will work — S3 + CloudFront, nginx, Apache, etc.

There is no `npm install`, no build step, and no environment configuration.

## Feature overview

- **Cinematic preloader** with a fast, animated progress reveal
- **Pinned hero-to-engineering-to-exploded-view journey** — one continuous scroll-driven sequence where the camera orbits the car, reveals six engineering hotspots with technical annotations, then the car explodes into its major components and reconstructs
- **Real-time 3D vehicle** built procedurally in Three.js (open-wheel chassis, exposed wheels, front/rear wings, cockpit halo, diffuser) with studio-style image-based lighting/reflections
- **Interactive configurator** — live exterior color, wheel style, and interior swaps with smoothly animated material transitions
- **Horizontal-scroll model collection**, scroll-driven performance counters, a procedural "speed lines" canvas sequence, and an interior cockpit hotspot diagram
- **Custom cursor, magnetic buttons, and micro-interactions** (desktop only)
- **Ambient sound toggle** using the Web Audio API — off by default, no autoplay
- **Fullscreen "cinematic mode"** toggle

## Responsiveness & fallbacks

- **Desktop (high core count):** full 3D scene with environment reflections in both the hero and the configurator.
- **Tablet / lower-end devices:** the hero 3D scene still renders; the configurator falls back to a lightweight SVG silhouette to save GPU budget.
- **No WebGL support:** the entire hero automatically falls back to an animated SVG silhouette so the page is never broken — it degrades, it doesn't fail.
- **`prefers-reduced-motion`:** disables the custom cursor, parallax, ambient light-sweep animations, and simplifies scroll reveals to a straightforward fade-in.
- **Mobile layout** is a dedicated composition (stacked stats, simplified hero) rather than a shrunk desktop layout, and avoids horizontal overflow.

## Browser support

Current versions of Chrome, Edge, Safari, and Firefox. Older browsers without WebGL or with JavaScript disabled will still see readable content and working navigation via the CSS/SVG fallback paths, but will miss the 3D and scroll-driven sequences.

## Customization

Everything content-related lives in the markup and a handful of clearly labeled JavaScript sections inside `index.html`:

| To change… | Look for… |
|---|---|
| Brand name / copy / taglines | The relevant `<section>` in the HTML body |
| Colors (paint, wheels) in the configurator | `#cfg-exterior`, `#cfg-wheels` swatch markup + `setCarColor()` / `setWheelStyle()` |
| Performance specs (HP, 0–100, top speed) | `data-target` attributes in the `#performance` section |
| Car geometry | `buildCarGroup()` |
| Color palette / typography scale | CSS custom properties at the top of the `<style>` block (`:root { --bg, --fg, --accent, ... }`) |
| Engineering hotspot content | The six `.hotspot` blocks inside `#machine` |

There's no data layer or build step to worry about — every edit is a direct change to the shipped file.

## Known constraints

- The 3D vehicle is an original **stylized, procedural** design (built from primitive/extruded geometry directly in code), not a licensed 3D model or photographic render. This was a deliberate choice to avoid depending on any copyrighted or trademarked vehicle design.
- Interior, cockpit, and material-swatch visuals are built from CSS/canvas rather than photography, for the same reason — no licensed imagery is used anywhere on the site.
- Ambient audio is a generated tone via the Web Audio API, not a licensed sound recording.

## License

This is a concept/demo project built around a fictional brand. Adapt freely for portfolio or client presentation purposes.
