# CLAUDE.md — Awttad Alqaser Building Contracting website

## What this is

A **bilingual static marketing website** for Awttad Alqaser Building Contracting
L.L.C, a licensed UAE residential construction company (new builds,
renovation/extension, interior fit-out).

- Plain HTML + CSS + vanilla JS. **No build tools, no frameworks, no npm, no
  package.json.** What is in the repo is exactly what ships.
- **Two pages**, both single-page with anchor navigation:
  - `index.html` — English, left-to-right. The default.
  - `ar/index.html` — Arabic, right-to-left (`<html lang="ar" dir="rtl">`).
  Both share the same `css/style.css`, `js/script.js`, images and video.
- Repo: `Kassabosko/AAQ` on GitHub. Deployed via **GitHub Pages** from the
  `main` branch to the custom domain **aaquae.com** (the `CNAME` file at the
  repo root sets that domain — do not delete or rename it).
- To preview locally, run the `aaq-preview` server in `.claude/launch.json`
  (a small PowerShell static server; `.claude/` is gitignored). Opening
  `index.html` directly as a file works too, but `/ar/` folder URLs will not
  resolve that way.

## Working preferences (important)

- **The owner is not a programmer.** Before making changes, explain in plain
  language what will change and what it will look like — no jargon-first
  explanations, no assuming familiarity with CSS terms.
- **Never run `git commit` or `git push` without asking first**, and show what
  changed before anything goes live. Pushing to `main` publishes to the public
  site immediately, so review comes first, every time.

## Bilingual setup

- The language switch is the gold-outlined `.lang-switch` button in the header,
  with a globe icon. English page → "العربية" → `ar/`. Arabic page → "English"
  → `../`. It is **never hidden at any screen size** — that is deliberate.
- Both pages carry `hreflang` alternate links in `<head>` so Google serves the
  right language.
- **Any content change must be made on both pages.** There is no shared
  template — that was the accepted trade-off for having real, indexable Arabic
  content rather than JavaScript text swapping.
- The Arabic translation was written by Claude and **should be reviewed by a
  native Arabic speaker**; treat it as good but unverified.

### Arabic-specific rules

- **Fonts** are swapped by redefining the tokens under `[dir="rtl"]`:
  **Cairo** for headings, **Tajawal** for body. Fraunces and Work Sans have no
  Arabic glyphs, so they are not used on the Arabic page. The Arabic page loads
  its own Google Fonts link.
- **Never apply `letter-spacing` or `text-transform: uppercase` to Arabic.**
  Arabic is a connected script — tracking breaks the joins — and it has no
  upper case. The `.eyebrow` and heading tracking are switched off under RTL.
- Arabic gets a looser `line-height` (1.8 body, 1.35 headings) for its
  ascenders and diacritics.
- The testimonial is **not** italic in Arabic (browsers fake it and it looks
  wrong).
- Latin-script runs inside Arabic (email, phone number) need `dir="ltr"` on the
  element or the punctuation reorders.

## Design system

### Colors (sampled from the actual company logo)

Defined once as CSS custom properties in `:root` at the top of `css/style.css`.
Always use the variables, never hard-code a new hex value.

| Variable | Value | Used for |
|---|---|---|
| `--gold` | `#D7A833` | Primary gold: buttons, process step circles, icon strokes, hero rule, language switch border |
| `--gold-dark` | `#B8901F` | Deeper gold: eyebrow text on light, hover states, stat numbers |
| `#E3CB93` | champagne | Hero video tint overlay only (literal value in `.hero-tint`) |
| `--charcoal` | `#2A2A28` | Body text, dark section backgrounds (process, contact) |
| `--charcoal-soft` | `#3D3D39` | Contact card backgrounds on dark |
| `--cream` | `#FBF7F1` | Warm cream page background and header bar |
| `--cream-deep` | `#F1E9D8` | Testimonial section background |
| `--white` | `#FFFFFF` | Services + Why sections background, hero headline, headings on dark |
| `--muted` | `#7A7266` | Body/secondary text |
| `--line` | `#E6DECB` | Hairline borders and dividers |

Other tokens: `--container-w: 1240px` (max content width), `--radius: 4px`
(corner rounding — deliberately subtle, keep it restrained).

### Fonts

Loaded from Google Fonts in each page's `<head>`.

- English — **Fraunces** (serif, `--font-display`) for `h1/h2/h3`, stat numbers,
  service numbers, process numbers and the testimonial quote; **Work Sans**
  (sans, `--font-body`) for body copy, buttons, navigation and `.eyebrow`.
- Arabic — **Cairo** (`--font-display`) and **Tajawal** (`--font-body`), swapped
  in under `[dir="rtl"]`.

Headings use `clamp()` for fluid sizing, so they scale with the viewport without
extra media queries.

### Recurring patterns

- `.eyebrow` — small uppercase gold label above each section heading
  (`.eyebrow-light` is the brighter gold variant for dark backgrounds). Uppercase
  and tracking are switched off in Arabic.
- `.btn` with `.btn-primary` (solid gold) or `.btn-ghost` (outlined).
- `.reveal` — add this class to any element that should fade/slide in on scroll.
  `js/script.js` watches for it with an IntersectionObserver and adds
  `.is-visible`. Animations are disabled under `prefers-reduced-motion`.
- `.full-photo` — makes an image fill its container with `object-fit: cover`.
- Section background rhythm alternates cream → white → charcoal → cream →
  cream-deep → white → charcoal, so a new section should pick a background that
  keeps that alternation from breaking.

## Page structure (same on both languages, in order)

1. **Header** — sticky, translucent cream with blur. Logo, nav, language switch,
   "Request a Quote" button, plus a hamburger toggle that only shows on mobile.
   **No phone number** — it was removed from the bar deliberately.
2. **Hero** — full-bleed autoplaying muted looping background video
   (`videos/hero-video.mp4`, poster `images/hero-poster.jpg`), a champagne gold
   tint at 26% opacity, a directional dark scrim, and the headline sitting
   **directly on the video** with a gold rule down its leading edge. There is no
   panel behind the text — that was removed on purpose; see below.
   Layer order is `z-index` 0→4 (media, video, tint, scrim, content).
3. **Intro / About** (`#about`) — company blurb plus three stats (15+ years,
   80+ homes, 100% licensed).
4. **Services** (`#services`) — three alternating image/text rows. The reversed
   row uses a `direction: rtl` trick on `.service-row.reverse`, which is
   **inverted again under RTL** so the rhythm matches the English page.
5. **Process** (`#process`) — dark charcoal band, four numbered steps joined by a
   thin gold connecting line (hidden on smaller screens).
6. **Projects** (`#projects`) — 2×2 photo grid with white caption chips (pinned
   to the right edge under RTL).
7. **Testimonial** — single centered quote (Petros Mavrakis).
8. **Why Choose Us** — four items, each with an inline SVG icon.
9. **Contact** (`#contact`) — dark band with email and phone cards. **There is no
   contact form** — contact is by `mailto:` and `tel:` links only, so the site
   needs no backend.
10. **Footer** — logo, short nav, copyright.

Contact details: **awttadalqaser@gmail.com** and **+971 56 917 9781**. These now
appear only in the contact cards and are duplicated across both language pages —
update all four places together.

## Conventions to follow

- `css/style.css` is one file, organized into clearly commented banner sections
  (TOKENS, BUTTONS, HEADER, HERO, INTRO/STATS, SERVICES, PROCESS, PROJECTS,
  TESTIMONIAL, WHY, CONTACT, FOOTER, SCROLL REVEAL, RTL/ARABIC, RESPONSIVE). Add
  new rules into the matching section, and keep the banner comment style.
- **All responsive rules live at the bottom** in the RESPONSIVE section, not
  scattered inline. Three breakpoints: `980px` (grids collapse), `860px` (mobile
  nav appears, logo shrinks) and `380px` (the header wraps onto two rows so the
  logo, language switch and quote button are never squashed).
- **All RTL overrides live in the RTL/ARABIC section**, keyed off `[dir="rtl"]`.
  Never fork the stylesheet.
- Icons are hand-written inline SVG with `stroke="#D7A833"` and
  `stroke-width="1.6"`. Match that style rather than adding an icon library.
- HTML is indented 2 spaces, sections separated by `<!-- ==== NAME ==== -->`
  comment banners.
- Accessibility habits already in place: `aria-label` on the logo, language
  switch and nav toggle, `aria-expanded` kept in sync on the toggle,
  `aria-hidden` on decorative icons, real `alt` text on photos. Keep these up.
- No dependencies beyond the Google Fonts links. Prefer solving things in plain
  CSS/JS rather than adding a library.

## Assets

| File | Size | Notes |
|---|---|---|
| `videos/hero-video.mp4` | 1.4 MB | 1280×720, H.264 CRF 30, no audio, faststart. Re-encoded down from 5.3 MB. |
| `images/hero-poster.jpg` | 203 KB | Shown before the video loads, and on phones that block autoplay. |
| `images/logo-header.png` | 851 KB | English logo, used in header and footer. **Oversized — worth compressing.** |
| `images/logo-header-ar.png` | 81 KB | Arabic logo. Background keyed to transparent and the phone number cropped off the original supplied file. |
| `images/logo.png` | 1.1 MB | **Not referenced by anything.** Safe to delete. |
| project / service photos | 170–310 KB each | Could be compressed. |

`ffmpeg` is installed on the owner's machine (via winget, `Gyan.FFmpeg`) and is
what was used for the video and the logo background removal.

## Known quirks / things worth knowing

- `.img-placeholder` in the CSS is **leftover and unused** — real photos replaced
  the placeholders. Safe to delete if tidying up.
- The hero's contrast comes from `.hero-scrim`, not from a solid panel. If the
  video is ever swapped for brighter footage, the scrim opacity is the value to
  revisit. Its gradient direction is mirrored under RTL.
- Phones in Low Power Mode / Data Saver refuse to autoplay video and will show
  `hero-poster.jpg` instead. That is browser policy, not a bug.
- The original 5.3 MB video blob is still in git history; this affects clone size
  only, not the live site.
