# AGENTS.md

## What is this

Single-page SvelteKit static site for Digim.i. All content lives in `src/routes/+page.svelte` (~90 lines): the `synonyms` tagline array (22 entries, click rotates), the `socialLinks` array (Instagram, YouTube, Threads), and a radial gradient whose centre follows the cursor. No backend, no tests, no CI.

## Commands

| Task | Command |
|------|---------|
| Dev server | `npm run dev` |
| Typecheck | `npm run check` |
| Build (static) | `npm run build` |
| Preview build | `npm run preview` |

No lint or format commands exist. No test suite.

## Key details

- **Svelte 5 runes mode** — components use `$state`, `$props`, `{@render}`. Not legacy Svelte 4 syntax.
- **Tailwind CSS v4** — configured via `@tailwindcss/vite` plugin and `@import "tailwindcss"` in `app.css`. No `tailwind.config.js`.
- **Static adapter** — outputs to `build/` with SPA fallback (`index.html`). Pages are prerendered (`prerender = true` in `+layout.ts`).
- **Font** — Space Grotesk loaded via CSS custom property `--font-display`.
- **Icons** — SVG paths are inlined as strings in the `socialLinks` array. Both `lucide-svelte` and `svelte-feather-icons` are installed but icons are hand-rolled inline in `+page.svelte`.
- **`.npmrc`** — `engine-strict=true` is set, but `package.json` has no `engines` field. This is harmless but unusual.
- **Video background** — `static/bg.mp4` (360×640, muted, loop, autoplay) sits behind a 70% opacity radial gradient scrim that follows the cursor. Video is blurred (`blur-sm`) and hidden under `prefers-reduced-motion`.
