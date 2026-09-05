# CLAUDE.md — Awttad Alqaser Building Contracting website

## What this is

A **static marketing website** for Awttad Alqaser Building Contracting L.L.C, a
licensed UAE residential construction company (new builds, renovation/extension,
interior fit-out).

- Plain HTML + CSS + vanilla JS. **No build tools, no frameworks, no npm, no
  package.json.** What is in the repo is exactly what ships.
- Single page, `index.html`, with anchor-link navigation (`#services`,
  `#process`, `#projects`, `#about`, `#contact`).
- Repo: `Kassabosko/AAQ` on GitHub. Deployed via **GitHub Pages** from the
  `main` branch to the custom domain **aaquae.com** (the `CNAME` file at the
  repo root sets that domain — do not delete or rename it).
- To preview locally, just open `index.html` in a browser, or serve the folder
  with any static server. There is nothing to compile.

## Working preferences (important)

- **The owner is not a programmer.** Before making changes, explain in plain
  language what will change and what it will look like — no jargon-first
  explanations, no assuming familiarity with CSS terms.
- **Never run `git commit` or `git push` without asking first**, and show what
  changed before anything goes live. Pushing to `main` publishes to the public
  site immediately, so review comes first, every time.

## Design system

### Colors (sampled from the actual company logo)

Defined once as CSS custom properties in `:root` at the top of `css/style.css`.
Always use the variables, never hard-code a new hex value.

| Variable | Value | Used for |
|---|---|---|
| `--gold` | `#D7A833` | Primary gold: buttons, process step circles, icon strokes, links on dark |
| `--gold-dark` | `#B8901F` | Deeper gold: eyebrow text on light, hover states, stat numbers, hero panel border |
| `#E3CB93` | champagne | Hero video tint overlay only (literal value in `.hero-tint`) |
| `--charcoal` | `#2A2A28` | Body text, dark section backgrounds (process, contact) |
| `--charcoal-soft` | `#3D3D39` | Contact card backgrounds on dark |
| `--cream` | `#FBF7F1` | Warm cream page background |
| `--cream-deep` | `#F1E9D8` | Testimonial section background |
| `--white` | `#FFFFFF` | Services + Why sections background, headings on dark |
| `--muted` | `#7A7266` | Body/secondary text |
| `--line` | `#E6DECB` | Hairline borders and dividers |

Other tokens: `--container-w: 1240px` (max content width), `--radius: 4px`
(corner rounding — deliberately subtle, keep it restrained).

### Fonts

Loaded from Google Fonts in the `<head>` of `index.html`.

- **Fraunces** (serif) — `--font-display`, used for `h1/h2/h3`, the stat numbers,
  service numbers, process numbers, and the testimonial quote (italic).
- **Work Sans** (sans-serif) — `--font-body`, used for all body copy, buttons,
  navigation, and the `.eyebrow` labels.

Headings use `clamp()` for fluid sizing, so they scale with the viewport without
extra media queries. `h1` is currently white with a text-shadow by default, but
inside the hero panel it is overridden to charcoal with no shadow.

### Recurring patterns

- `.eyebrow` — small uppercase gold label above each section heading
  (`.eyebrow-light` is the brighter gold variant for dark backgrounds).
- `.btn` with `.btn-primary` (solid gold) or `.btn-ghost` (outlined).
- `.reveal` — add this class to any element that should fade/slide in on scroll.
  `js/script.js` watches for it with an IntersectionObserver and adds
  `.is-visible`. Animations are disabled under `prefers-reduced-motion`.
- `.full-photo` — makes an image fill its container with `object-fit: cover`.
- Section background rhythm alternates cream → white → charcoal → cream →
  cream-deep → white → charcoal, so a new section should pick a background that
  keeps that alternation from breaking.

## Page structure (`index.html`, in order)

1. **Header** — sticky, translucent cream with blur. Logo, nav, phone number,
   "Request a Quote" button, plus a hamburger toggle that only shows on mobile.
2. **Hero** — full-bleed autoplaying muted looping background video
   (`videos/hero-video.mp4`, poster `images/hero-poster.jpg`), a champagne gold
   tint layer at 60% opacity, a dark gradient scrim, and a semi-transparent
   white text panel with a gold left border holding the headline and two buttons.
   Layer order is controlled by `z-index` 0→4 (media, video, tint, scrim, content).
3. **Intro / About** (`#about`) — company blurb plus three stats (15+ years,
   80+ homes, 100% licensed).
4. **Services** (`#services`) — three alternating image/text rows. The reversed
   row uses a `direction: rtl` trick on `.service-row.reverse` to flip the
   columns, with children reset to `ltr`.
5. **Process** (`#process`) — dark charcoal band, four numbered steps joined by a
   thin gold connecting line (hidden on smaller screens).
6. **Projects** (`#projects`) — 2×2 photo grid with white caption chips.
7. **Testimonial** — single centered italic quote (Petros Mavrakis).
8. **Why Choose Us** — four items, each with an inline SVG icon.
9. **Contact** (`#contact`) — dark band with email and phone cards. **There is no
   contact form** — contact is by `mailto:` and `tel:` links only, so the site
   needs no backend.
10. **Footer** — logo, short nav, copyright.

Contact details used throughout: **awttadalqaser@gmail.com** and
**+971 56 917 9781** (each appears in more than one place — update all of them
together).

## Conventions to follow

- `css/style.css` is one file, organized into clearly commented banner sections
  (TOKENS, BUTTONS, HEADER, HERO, INTRO/STATS, SERVICES, PROCESS, PROJECTS,
  TESTIMONIAL, WHY, CONTACT, FOOTER, SCROLL REVEAL, RESPONSIVE). Add new rules
  into the matching section, and keep the banner comment style.
- **All responsive rules live at the bottom** in the RESPONSIVE section, not
  scattered inline. Two breakpoints only: `980px` (grids collapse) and `860px`
  (mobile nav appears, hero video hidden in favor of the poster image).
- Icons are hand-written inline SVG with `stroke="#D7A833"` and
  `stroke-width="1.6"`. Match that style rather than adding an icon library.
- HTML is indented 2 spaces, sections separated by `<!-- ==== NAME ==== -->`
  comment banners.
- Accessibility habits already in place: `aria-label` on the logo and nav
  toggle, `aria-expanded` kept in sync on the toggle, `aria-hidden` on decorative
  icons, real `alt` text on photos. Keep these up in new markup.
- No dependencies beyond the Google Fonts link. Prefer solving things in plain
  CSS/JS rather than adding a library.

## Known quirks / things worth knowing

- `.img-placeholder` in the CSS is **leftover and unused** — real photos replaced
  the placeholders. Safe to delete if tidying up.
- `.hero-copy` has a light color rule for use over video, then a darker override
  inside `.hero-panel`. If the hero panel is ever removed, the copy colour needs
  rechecking.
- `images/logo.png` (1.1 MB) is not referenced by the page; `images/logo-header.png`
  (871 KB) is the one actually used, in both header and footer.
- **Asset sizes are heavy**: the hero video is 5.4 MB and several images are
  200–300 KB each. Compressing these is the single biggest available speed
  improvement, especially for mobile visitors on the UAE mobile network.
- The hero video is hidden below 860px wide, so mobile visitors see the poster
  image instead — check `images/hero-poster.jpg` whenever the video changes.
