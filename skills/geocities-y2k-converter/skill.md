---
name: geocities-y2k-converter
description: Convert any website into a switchable year-2000 GeoCities / Y2K-Futurism retro theme (chrome metallic, glossy 3D blobs, holographic gradients, marquees, blink, hit counters, web rings, guestbooks) WITHOUT destroying the existing modern design. Use when the user asks to add a retro/GeoCities/Y2K/Web-1.0/2000s look, a nostalgia mode, or a "switch to the 90s/2000s" toggle to a site or component. Works for Next.js/React, plain HTML/CSS, Vue, and similar stacks.
---

# GeoCities / Y2K Converter

Turn a target website into an authentic year-2000 personal-homepage look that can be
**toggled on and off** from a control in the page, while the original modern design stays
fully intact. The retro look layers GeoCities Web 1.0 amateur charm with Y2K-Futurism
polish (chrome, gloss, holographic gradients).

The user gives you a site or component to convert. Inspect it, then add the retro variant
as an *additive* layer — never delete or rewrite the existing theme.

## Design reference (ground every choice in this)

**Palette** — silver/chrome base with aggressive neon pops:
- Silver / chrome `#c0c0c0`, light chrome `#f2f3f4`
- Electric blue `#1493ff`, lime `#7fff00`, hot pink `#ff1493`, purple `#8a2be2`, orange `#ffa500`
- Classic GeoCities navy background `#000080`, neon yellow/cyan text, magenta visited links

**Materials & effects:**
- Chrome / mirror-finish metallic text (vertical silver gradient clipped to text)
- Glossy, inflated 3D "blob" buttons with a bright top highlight (balloon-like)
- Holographic / iridescent gradient bars (the "sheen")
- Beveled `ridge`/`outset`/`inset` borders, tiled starfield background, animated rainbow `<hr>`

**Typography:**
- Body: Comic Sans MS (the amateur GeoCities signal)
- Display/titles: geometric techno faces — Orbitron / Eurostile / Bank Gothic lineage (with fallbacks)
- Monospace (Courier New) for counters and 88x31 buttons

**Iconic UI elements to include:** `<marquee>` banners, blinking text, "Under Construction"
badge, hit/visitor counter, 88x31 web-ring buttons, "Cool Links" bullet list, guestbook +
"webmaster@" mailto, "Best viewed in Netscape Navigator 800x600".

> Note: pure Y2K-Futurism (chrome/cyber) was *succeeded* by **Frutiger Aero** (~2004-2013,
> glossy aqua-bubble / nature-tech). If the user wants that instead, shift the palette to
> sky-blues, water/grass imagery and even glossier glass — but it is a distinct aesthetic.

## Bundled assets

- `assets/geocities.css` — portable, framework-agnostic stylesheet. Every class is scoped
  under `.geocities-page` so it never leaks into the modern theme. Copy it into the project
  (or import it) rather than re-deriving the styles.
- `assets/template.html` — a complete reference page showing every building block with
  `{{PLACEHOLDER}}` slots; ideal starting point for plain-HTML sites.

## Conversion process

### 1. Inspect the target
- Identify the stack (Next.js/React, Vue, plain HTML, etc.), the entry page/component, and
  any existing theme mechanism (e.g. a ThemeContext, a `data-theme` attribute, a CSS class
  on `<html>`/`<body>`).
- Locate the site's real content (titles, tagline, links, social profiles, contact email).
  **Reuse existing content / i18n strings** — do not invent copy.

### 2. Add a persisted "retro" toggle (additive, never destructive)
Add a boolean mode alongside the existing theme, persisted in `localStorage`, that adds a
`geocities` class to the root element. Keep the modern dark/light theme untouched.

React/Next example (extend the existing theme context):
```tsx
const [retro, setRetro] = useState(false);
useEffect(() => {
  if (localStorage.getItem('retro') === 'true') {
    setRetro(true);
    document.documentElement.classList.add('geocities');
  }
}, []);
const toggleRetro = () => {
  const next = !retro;
  setRetro(next);
  localStorage.setItem('retro', String(next));
  document.documentElement.classList.toggle('geocities', next);
};
```
Plain HTML: a `<button>` that toggles `body.classList`/`localStorage` in a small inline script.

### 3. Build a self-contained retro view
Render a dedicated retro page/component when the mode is active; return early so the modern
component never mounts:
```tsx
if (retro) return <GeoCitiesHome />;
```
Build `GeoCitiesHome` (or the equivalent) from `assets/template.html`, wiring the target's
real content into the slots. Put a control bar at the top (`.gc-nav`) with a "Return to the
Future" button (calls `toggleRetro`) and the language switch if the site is multilingual.

### 4. Wire up the styles
Copy `assets/geocities.css` into the project and import it (Next.js: import in `globals.css`
or the layout; plain HTML: `<link rel="stylesheet">`). Apply the classes: `.geocities-page`
on the root container, `.gc-panel`, `.gc-title`, `.gc-chrome`, `.gc-holo`/`.gc-hr`,
`.gc-button88`, `.gc-gloss-btn` (`.pink`/`.lime`), `.gc-link-list`, `.gc-counter`,
`.gc-construction`, `.blink`, `.gc-rainbow`.

### 5. Add the toggle to the modern site's controls
Add a clearly-labelled button (e.g. `💾 90s`) to the existing top controls so users can
enter retro mode from the normal site.

## Mobile friendliness (required)
The bundled CSS is already responsive — preserve it:
- Panels use `width: calc(100% - 24px)` + `box-sizing: border-box` so they never overflow.
- Titles/headings use `clamp()`; the `@media (max-width: 600px)` block stacks glossy buttons
  full-width, wraps the nav, and shrinks the starfield.
- Ensure a `<meta name="viewport" content="width=device-width, initial-scale=1">` exists
  (Next.js app router injects this automatically).
- Keep the `prefers-reduced-motion` block so animations can be disabled.
Verify at 320px, 375px and 768px widths — no horizontal scroll, no clipped text.

## Framework gotchas
- **React 19 removed `<marquee>` from JSX intrinsics.** Add a type declaration, or render a
  CSS-based marquee instead. Declaration:
  ```ts
  // types/jsx-marquee.d.ts
  import 'react';
  declare module 'react' {
    namespace JSX {
      interface IntrinsicElements {
        marquee: React.DetailedHTMLProps<
          React.HTMLAttributes<HTMLElement> & { scrollamount?: number | string; direction?: string; behavior?: string },
          HTMLElement
        >;
      }
    }
  }
  ```
- External links: always `target="_blank" rel="noopener noreferrer"`.
- SSR/hydration: default `retro` to `false` and flip it in an effect (matches typical theme
  hydration); accept the one-frame flash or gate render on a `mounted` flag.
- Don't import webfonts the build can't fetch in a sandbox — reference Orbitron/Eurostile by
  name with safe fallbacks so it degrades gracefully.

## Success criteria
- ✅ A visible toggle switches into retro mode and back; choice persists across reloads.
- ✅ The original modern design is completely unchanged when retro is off.
- ✅ Retro view shows the site's *real* content with the iconic Y2K/GeoCities elements.
- ✅ Fully responsive (no overflow at 320px) and respects `prefers-reduced-motion`.
- ✅ Project builds, type-checks and lints clean.
