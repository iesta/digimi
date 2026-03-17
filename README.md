# Digim.i

A minimal static homepage for electronic noise experimentation.

![Digim.i](https://img.shields.io/badge/Digim.i-2d3d4d?style=flat&color=2d3d4d)

## About

Digim.i is a simple, elegant landing page featuring:

- 🎵 Random rotating taglines describing electronic music experimentation
- 📸 Social links (Instagram, YouTube, Threads)
- 🎨 Dark grey radial gradient background
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

- Social links in the `socialLinks` array
- Taglines in the `synonyms` array
- Colors in the inline gradient style

## License

MIT
