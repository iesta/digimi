# Digim.i

A minimal static homepage for electronic noise experimentation.

![Digim.i](https://img.shields.io/badge/Digim.i-2d3d4d?style=flat&color=2d3d4d)

## About

Digim.i is a simple, elegant landing page featuring:

- 🎬 Seven video backgrounds with slowed audio (`static/bg.mp4` … `bg-7.mp4`), 24 fps, 0.8× speed, unmute button beside the icons, chained one after the other under a CSS effect stack: scanlines, grain, tracking band, vignette, and a cursor-following gradient scrim
- 🎵 Random rotating taglines (22) describing electronic music experimentation
- 📸 Social links (Instagram, YouTube, Threads)
- 🎨 Dark grey radial gradient background that follows the cursor
- ✨ Smooth hover animations

## Tech Stack

- **SvelteKit** - Static site generation
- **Tailwind CSS** - Styling
- **Space Grotesk** - Typography
- **Static adapter** - Zero backend required

## Getting Started

```bash
# Clone the repo
git clone https://github.com/iesta/digimi.git
cd digimi

# Install dependencies
npm install

# Start development server
npm run dev
```

## Building

```bash
# Build static site
npm run build

# Preview production build
npm run preview
```

## Customization

Edit `src/routes/+page.svelte` to change:

- Social links, names, and inline SVG icons in the `socialLinks` array
- Taglines in the `synonyms` array (22 today; click rotates, never repeating the current one)
- Gradient colours in the inline `radial-gradient` style (centre tracks the cursor via `mousemove`)

## License

MIT
