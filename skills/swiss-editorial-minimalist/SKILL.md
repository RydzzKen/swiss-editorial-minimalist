---
name: swiss-editorial-minimalist
owner: RydzzKen
description: >-
  Swiss-Editorial Minimalist web design, v3. Art direction for websites that read like a printed
  magazine made by engineers: monochrome palette, large display type with negative tracking, visible
  borders and numbered sections, no shadows and no bright colours. Use this whenever you build,
  redesign or polish a landing page, company profile, portfolio, dev-tool or AI startup page, or any
  public page that should look "minimalist brutalist", "Swiss style", "editorial" or "monochrome".
  Includes the house rules, the anti-pattern list, and a checklist to run before delivery.
---

# swiss-editorial-minimalist (SKILLS.md v3.1)

A page built by a model usually looks like the average of every page it has seen: a gradient hero, a
row of rounded cards with emoji icons, a shadow under every button, and a Tailwind CDN that loads a
rainbow of colours. Nothing in it is wrong, and nothing in it was decided.

This file replaces those defaults with decisions. It covers three things: the house style (what to
show and what to leave out), the technical rules that make it hold (tokens, scales, breakpoints,
accessibility), and the gates that check the result before it ships.

Follow the rules as written. Where a request conflicts with them, section 0 says what to do.

## 0. Meta rules (read first)

### What to do when you receive this file

1. **Read the whole file before writing any code.** The rules depend on each other.
2. **Do not ask for clarification on anything this file already decides.** Execute with reasonable
   assumptions and state them in one line.
3. **If the request conflicts with the house style** (for example "colourful", "playful", "gradient
   hero", "emoji icons", "20px rounded cards"), confirm once before you proceed:
   > This request breaks section X of SKILLS.md. I will interpret it in Swiss-editorial Minimalist
   > style, unless you want to leave the guide.

   Wait for the answer, then build.
4. **Default output is one complete HTML file** with inline `<style>` and `<script>`. Do not deliver
   fragments unless the user asks for them.
5. **Always start the CSS with the `:root` block from section 12.**

### How to name this style

Use these terms so the vocabulary stays consistent. Do not use "modern", "clean" or "sleek" on their
own; they say nothing specific.

- **"Swiss-editorial Minimalist"**: the default, described in this file.
- **"Soft Brutalism"**: thicker borders and a rougher grid.
- **"Neo-Brutalist Editorial"**: one accent colour added and a hard offset shadow. Shadows are banned
  in the default style, so use this name only when the user explicitly asks for it.
- **"Monochrome Magazine"**: typography-led, with no illustrated cards.

---

## 1. Design philosophy

Three principles, none of them negotiable:

1. **Content is the king; typography is the architecture.** There are no decorative illustrations.
   The large heading is the main visual element.
2. **Structure stays visible.** Borders, grids and section numbers appear as they are. They are not
   hidden.
3. **Monochrome first, colour later.** The default is black, white and grey. Colour appears only in
   dark mode or as a minimal accent.

The page should feel calm, firm, smart, and editorial, like a printed magazine made by engineers.

---

## 2. Palette

### Light mode (default)

```css
:root {
  --bg:     #f5f5f5; /* main background, not pure white */
  --ink:    #111111; /* primary text, not pure black */
  --muted:  #666666; /* secondary text */
  --line:   #e0e0e0; /* borders and dividers */
  --card:   #eeeeee; /* card background */
  --accent: #d4d4d4; /* neutral accent */
  --dark:   #0a0a0a; /* high-contrast sections (statement, footer) */
  --white:  #ffffff;
}
```

### Dark mode

```css
body.dark {
  --bg:     #121212;
  --ink:    #f0f0f0;
  --muted:  #999999;
  --line:   #333333;
  --card:   #1e1e1e;
  --accent: #2a2a2a;
  --dark:   #050505;
  --white:  #181818;
}
```

### Colour rules (mandatory)

- **Body background is never `#fff`.** Use off-white `#f5f5f5`.
- **Text is never `#000`.** Use `#111`.
- **The brand colour appears one or two times per page at most**, and always through a CSS variable,
  never hardcoded.
- **Bright colours are banned** for status, icons and text. Examples of what is banned: emerald-500,
  green-600, red-500, blue-500, yellow-400, `#27c93f`, `#ff5f56`, `#ffbd2e`. Replace them with
  monochrome symbols (●, ▲, ✕, →) in `var(--ink)` or `var(--muted)`.
- **Contrast sections** (statement, CTA, footer) use `var(--dark)` with light text.
- **Dark mode is required.** Follow `prefers-color-scheme` by default and let the user override it,
  saving the choice in `localStorage`. The toggle uses the mandatory markup in section 5
  ("Theme toggle").

---

## 3. Typography

### Font pairing

```
Heading:  'Space Grotesk'  (or Inter Tight, General Sans, Neue Montreal)
Body:     'DM Sans'        (or Inter, Satoshi, IBM Plex Sans)
Mono:     'JetBrains Mono' (or system monospace)
```

Do not use decorative, script or serif fonts for body text. A serif is allowed only for quotes or
signatures (for example Georgia, italic).

### Type scale (clamp is required, so the type scales without media queries)

```css
/* Hero H1 */
font-size: clamp(55px, 5.6vw, 94px);   /* or clamp(32px, 4.3vw, 62px) for a narrow hero */
line-height: 1.025;
letter-spacing: -0.07em;               /* negative, always */
font-weight: 500;                      /* not 700, not 400 */

/* Section H2 */
font-size: clamp(42px, 4.2vw, 69px);
line-height: 1.08;
letter-spacing: -0.06em;
font-weight: 500;

/* Card H3 */
font-size: clamp(22px, 2.2vw, 27px);
line-height: 1.2;
letter-spacing: -0.04em;
font-weight: 500;

/* Body */
font-size: 16px–18px;
line-height: 1.7–1.8;
color: var(--muted);

/* Micro / eyebrow / label */
font-size: 12px;
letter-spacing: 0.15em–0.17em;
font-weight: 700;
text-transform: uppercase;
```

### Hard typography rules

- Headings always use negative letter-spacing (between -0.04em and -0.07em). This is the signature
  of the style.
- Heading weight is 500. Bold 700 flattens the editorial feel.
- Eyebrows and micro-labels are always UPPERCASE with wide tracking (0.15em or more).
- Section numbers (01, 02, 03) appear as micro-labels. Do not hide them.
- A full stop at the end of a headline can serve as an accent, as an option:
  ```html
  <h1>Judul.<span style="color:var(--muted)">.</span></h1>
  ```
- Body text is left-aligned. Do not centre long paragraphs.

---

## 4. Layout and grid

### Container

```css
.wrap {
  max-width: 1440px;
  margin: auto;
  padding: 0 clamp(24px, 5vw, 84px);
}
```

### Vertical spacing

- Between sections: `padding: 130px 0` on desktop, reducing to `85px` on mobile.
- Header height: 88px on desktop, 75px on mobile.
- Gap between cards: 15–16px. Keep it tight, not 24px or more.

### Common grid patterns

```css
/* Asymmetric hero */
.hero { grid-template-columns: 1.04fr .96fr; }

/* Two-column section head */
.section-head { grid-template-columns: 1fr 1fr; align-items: end; }

/* Three-column features */
.features { grid-template-columns: repeat(3, 1fr); gap: 15px; }

/* Statement */
.statement-grid { grid-template-columns: 1fr 1fr; gap: 60px; }

/* Testimonial (see section 8) */
.testimonial-grid { grid-template-columns: 1.2fr .8fr; gap: 60px; }
```

### Layout rules (mandatory)

- Sections are separated by a 1px `var(--line)` border, not by a shadow or a different background.
- Avoid large border-radius. Buttons and cards get at most 4px. Only images and screenshots take
  12–22px.
- Use `align-items: end` on section heads so the heading and paragraph sit on the same baseline. This
  is the editorial trick.
- Asymmetric grids (1.04fr / .96fr) look better than 1fr / 1fr.
- Never centre body text. Always left-align.
- Every section needs a `border-top` or `border-bottom` of 1px `var(--line)`.

---

## 5. Components

### Buttons

```css
.btn {
  display: inline-flex;
  align-items: center;
  gap: 14px;
  border-radius: 4px;                 /* almost square */
  padding: 15px 20px;
  background: var(--ink);
  color: var(--bg);
  border: 1px solid var(--ink);
  font-size: 14px;
  font-weight: 700;
  cursor: pointer;
  transition: transform .2s, background .2s;
}
@media (hover: hover) {
  .btn:hover { transform: translateY(-2px); background: #333333; }
}

/* Variants */
.btn.outline { background: transparent; color: var(--ink); border-color: var(--line); }
.btn.light   { background: var(--bg); color: var(--ink); border-color: var(--bg); }
```

Button rules:

- Every button has an arrow icon: ↗ for an external link, ↓ for a scroll.
- Hover lifts the button by -2px, only inside `@media (hover: hover)`. It does not scale and does not
  add a shadow.
- Never use `#fff` for `.btn.light`. Use `var(--bg)`.

### Feature cards

```
┌─────────────────────────┐
│  [SVG line-art 8:5]     │  ← abstract monochrome illustration, or empty
├─────────────────────────┤
│  LABEL MICRO            │  ← uppercase, wide tracking
│  Judul kartu.           │  ← Space Grotesk 24–27px
│  Deskripsi 1–2 kalimat  │  ← muted, 14px
└─────────────────────────┘
```

- Background `var(--card)`, no border-radius (or 4px at most).
- No border (the card blends into the background), or `1px solid var(--line)`.
- Hover: `transform: translateY(-5px)`, only inside `@media (hover: hover)`:
  ```css
  @media (hover: hover) { .card:hover { transform: translateY(-5px); } }
  ```
- The illustration is SVG line-art with `stroke-width: 1.2px`, `fill: none`, in the ink colour. Not a
  photo, not a gradient, not an emoji.

### Header

- Height 88px, `border-bottom: 1px solid var(--line)`.
- Brand: 34px logo and the name in Space Grotesk 22px, `letter-spacing: -1.1px`.
- Nav: 14px, weight 600, gap 36px.
- Actions: theme toggle (sun/moon icon, mandatory markup in "Theme toggle" below), the CTA button,
  and the hamburger on mobile.
- Mobile nav opens as an absolute panel below the header and closes on Escape.
- The hamburger needs `aria-label`, `aria-expanded` and `aria-controls`. The theme toggle needs
  `aria-label` and `aria-pressed` (see below).

### Theme toggle (mandatory markup)

Every page has a dark mode toggle, and it always uses this markup. Do not redraw the icons, swap them
for emoji, or add a text label. The SVG paths, the `id` values and the `aria-label` are fixed.

```html
<div class="header-actions">
  <!-- Dark Mode Toggle Button -->
  <button id="theme-toggle" type="button" class="theme-toggle" aria-label="Ganti mode gelap/terang" aria-pressed="false">
    <!-- Sun Icon (hidden in light mode, visible in dark mode) -->
    <svg id="sun-icon" class="theme-icon" fill="none" stroke="currentColor" viewBox="0 0 24 24" stroke-width="1.8" aria-hidden="true">
      <circle cx="12" cy="12" r="5"></circle>
      <path d="M12 1v2m0 18v2M4.22 4.22l1.42 1.42m12.72 12.72l1.42 1.42M1 12h2m18 0h2M4.22 19.78l1.42-1.42M18.36 5.64l1.42-1.42"></path>
    </svg>
    <!-- Moon Icon (visible in light mode, hidden in dark mode) -->
    <svg id="moon-icon" class="theme-icon" fill="none" stroke="currentColor" viewBox="0 0 24 24" stroke-width="1.8" aria-hidden="true">
      <path d="M21 12.79A9 9 0 1111.21 3 7 7 0 0021 12.79z"></path>
    </svg>
  </button>
  <!-- CTA button and hamburger go here, inside the same wrapper -->
</div>
```

```css
.header-actions { display: flex; align-items: center; gap: 16px; }

.theme-toggle {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  min-width: 44px;                    /* tap target */
  min-height: 44px;
  padding: 8px;
  color: var(--ink);
  background: transparent;
  border: 1px solid var(--line);
  border-radius: var(--radius-sm);
  cursor: pointer;
  transition: background .2s;
}
@media (hover: hover) {
  .theme-toggle:hover { background: var(--card); }
}
.theme-icon { width: 20px; height: 20px; }

/* Icon state follows body.dark, so no class swapping in JavaScript */
#sun-icon  { display: none; }
#moon-icon { display: block; }
body.dark #sun-icon  { display: block; }
body.dark #moon-icon { display: none; }
```

```html
<!-- First element inside <body>, so the saved theme applies before first paint -->
<script>
  (function () {
    var KEY = 'theme';
    var saved = null;
    try { saved = localStorage.getItem(KEY); } catch (e) {}
    var dark = saved ? saved === 'dark'
                     : window.matchMedia('(prefers-color-scheme: dark)').matches;
    document.body.classList.toggle('dark', dark);

    document.addEventListener('DOMContentLoaded', function () {
      var btn = document.getElementById('theme-toggle');
      if (!btn) return;
      btn.setAttribute('aria-pressed', String(document.body.classList.contains('dark')));
      btn.addEventListener('click', function () {
        var isDark = document.body.classList.toggle('dark');
        btn.setAttribute('aria-pressed', String(isDark));
        try { localStorage.setItem(KEY, isDark ? 'dark' : 'light'); } catch (e) {}
      });
    });
  })();
</script>
```

Theme toggle rules:

- Keep the wrapper (`.header-actions`), the button `id="theme-toggle"`, and the icon ids `sun-icon` and
  `moon-icon` exactly as written. Scripts and the checklist look for them.
- Moon shows in light mode (click to go dark). Sun shows in dark mode (click to go light). The default
  state is light, unless `prefers-color-scheme: dark` or a saved choice says otherwise.
- The button takes its colour from variables only: `var(--ink)` for the icon and text, `var(--line)` for
  the border, `var(--card)` for the hover background. Never hardcode a colour.
- The border-radius is 4px at most, the tap target is at least 44×44px, and the hover state sits inside
  `@media (hover: hover)`.
- The icons are 20px, `stroke="currentColor"`, `stroke-width="1.8"`, `fill="none"`, with
  `aria-hidden="true"`. The button carries the `aria-label`.
- Use `aria-pressed` on this button. `aria-expanded` is for the hamburger, not for the theme toggle.
- Save the choice in `localStorage` under the key `theme`, with values `light` or `dark`. Wrap storage
  calls in `try/catch`.
- The toggle works without Tailwind. The original snippet used Tailwind utility classes
  (`flex items-center gap-4`, `p-2`, `w-5 h-5`, `hidden`/`block`), and those are replaced here by the
  vanilla CSS above, because the Tailwind CDN is banned (section 10). If a project already runs
  Tailwind with a build step, the utility classes may stay, but the colour must be written
  `text-[var(--ink)]`. The form `text.var(--ink)` is a typo and does nothing.

### Footer

- Background `var(--dark)`, light text.
- Grid: brand and description on the left, two link columns on the right.
- Bottom bar: `border-top: 1px solid #333333`, with the copyright and "Semua hak dilindungi".

### FAQ (accordion)

- Use native `<details>` and `<summary>`. No JavaScript.
- Summary: flex with `justify-content: space-between`, Space Grotesk weight 500 at 18–22px.
- `::after` shows "+" when closed and "−" (unicode `\2212`) when open.
- `border-bottom: 1px solid var(--line)` on each item, with no background.

### Code block

- Tab header (curl / Python / JS) and a copy button.
- Background `#0a0a0a`, with manual syntax highlighting through `<span>` classes.
- No box-shadow. Use `border: 1px solid #222222`.
- `tabindex="0"` on the `<pre>` for keyboard access.
- The copy button has an `aria-label`.
- Do not add macOS traffic-light dots to the header (see section 10).

---

## 6. Animation and interaction

The principle is gentle, minimal motion that never gets in the way.

```css
/* Fade in on scroll */
.rv {
  opacity: 0;
  transform: translateY(20px);
  transition: opacity .6s ease, transform .6s ease;
}
.rv.in {
  opacity: 1;
  transform: none;
}

/* Hero image */
@keyframes appear {
  from { opacity: 0; transform: scale(1.06); }
  to   { opacity: 1; transform: scale(1); }
}
```

Animation rules:

- Reveal on scroll uses `IntersectionObserver` with `threshold: 0.12`.
- Hover moves only: `translateY` (-2px for buttons, -5px for cards). No shadow, blur or glow. Every
  hover rule sits inside `@media (hover: hover)` so touch screens do not get stuck hover states.
- `prefers-reduced-motion: reduce` is required. It turns off every animation and transition:
  ```css
  @media (prefers-reduced-motion: reduce) {
    *, *::before, *::after {
      animation: none !important;
      transition: none !important;
      scroll-behavior: auto !important;
    }
    .rv { opacity: 1 !important; transform: none !important; }
  }
  ```

---

## 7. Accessibility (mandatory)

- `:focus-visible { outline: 3px solid #888888; outline-offset: 4px; }` on every link and button.
- A skip link: `<a class="skip" href="#top">Lewati ke konten</a>`, visible when focused.
- Every image has `alt`, or `alt=""` when it is decorative.
- Decorative icons get `aria-hidden="true"`.
- The hamburger has `aria-label`, `aria-expanded` and `aria-controls`. The theme toggle has
  `aria-label` and `aria-pressed`.
- Text contrast is at least 4.5:1. `--muted #666` on `--bg #f5f5f5` passes.
- Large sections use the `<main>`, `<nav>`, `<header>` and `<footer>` landmarks.
- Accordions use `<details>`/`<summary>`, not a `div` with JavaScript.
- Icon-only buttons always have `aria-label`.

---

## 8. Page structure (standard section order)

A landing page in this style usually follows this order:

1. **Header**: brand, nav, theme toggle, CTA.
2. **Hero**: asymmetric grid. Copy on the left (eyebrow, H1, subhead, two buttons), visual on the
   right. The visual may be a `var(--dark)` panel with white SVG, or a light editorial card with ink
   SVG. Pick one and stay consistent.
3. **Strip**: a row of short feature labels (Chat · Web · Gambar · File · Kode), with borders above and
   below.
4. **Capabilities**: a 3×2 grid of feature cards with SVG illustrations.
5. **How it works**: three horizontal steps, each with a right border.
6. **Audience / use cases**: a 3-card grid, one card per persona.
7. **Showcase**: a real screenshot (phone mockup or image) with a small feature grid.
   - **7b. Testimonial**: a 2-column grid. A large quote on the left, left-aligned, in the heading font,
     optionally italic, with attribution. An SVG diagram or pattern on the right. Never centre it. If
     there is only one testimonial, keep the 2-column grid and put the decorative diagram on the right.
8. **Developer**: a code block, an architecture SVG diagram, and a grid of API cards.
9. **Status / transparency**: three cards labelled "Sudah jalan / Batasan / Belum ada".
10. **Founder**: 2-column grid with a sticky visual, copy, quote and signature.
11. **Social band**: a follow CTA and social channel buttons.
12. **FAQ**: native `<details>` accordion.
13. **Closing CTA**: grey section, large headline and button.
14. **Footer**: dark, with brand, link columns and copyright.

Number every section in its eyebrow (01, 02, 03…).

---

## 9. Responsive breakpoints

```css
/* Tablet */
@media (max-width: 900px) {
  header { height: 75px; }
  nav { display: none; }              /* replaced by hamburger */
  nav.open { display: flex; flex-direction: column; }
  .menu { display: block; }
  .hero, .section-head, .statement-grid,
  .testimonial-grid { grid-template-columns: 1fr; }
  .features { grid-template-columns: 1fr 1fr; }
}

/* Mobile */
@media (max-width: 600px) {
  .wrap { padding: 0 23px; }
  h1 { font-size: clamp(50px, 12vw, 70px); }
  .features, .principles { grid-template-columns: 1fr; }
  section.block { padding: 85px 0; }
  .foot-top, .foot-bottom { flex-direction: column; }
}
```

Responsive rules:

- `clamp()` handles about 80% of responsiveness. Avoid media queries for font sizes.
- On mobile, the nav opens as an absolute panel below the header. It closes on Escape and when a link
  is clicked.
- Every grid of two or more columns collapses to one column at 900px (tablet) or 600px (mobile). The
  feature grid may stay at two columns on tablet.

### Viewport and touch

Every page declares the viewport in `<head>`. Without it, the media queries above do not work on phones:

```html
<meta name="viewport" content="width=device-width, initial-scale=1, viewport-fit=cover">
```

```css
*, *::before, *::after { box-sizing: border-box; }

/* Wide content scrolls inside its own container, never the page */
img, video { max-width: 100%; height: auto; }
svg { max-width: 100%; }              /* do not set height:auto, it can collapse icons */
pre, .table-wrap, .diagram { overflow-x: auto; max-width: 100%; }

/* Notch and system bars (iOS). The inset is added to the header height. */
header {
  height: calc(88px + env(safe-area-inset-top, 0px));
  padding-top: env(safe-area-inset-top, 0px);
}
@media (max-width: 900px) {
  header { height: calc(75px + env(safe-area-inset-top, 0px)); }
}
footer { padding-bottom: env(safe-area-inset-bottom, 0px); }
```

Additional responsive rules:

- **Hover** rules (buttons, cards) always sit inside `@media (hover: hover)`. See Sections 5 and 6.
- **Tap targets**: buttons, nav links and the hamburger are at least 44×44px, with at least 8px between
  neighbours.
- **No horizontal page scroll** at any width from 320px up. Code blocks, tables and SVG diagrams scroll
  inside their own container (`overflow-x: auto`) and keep `tabindex="0"`.
- **Images** always have `max-width: 100%`. Give them `width`/`height` attributes or `aspect-ratio` to
  avoid layout shift.
- **Fixed or sticky elements** (header, cookie bar) never cover more than 20% of the mobile viewport
  height.
- **Body text** stays at 16px or more on mobile, so iOS does not zoom on focus. Form inputs also use
  16px or more.
- **Test widths**: 360px (small phone), 768px (tablet), 1280px (laptop) and 1440px+ (wide). Check both
  light and dark theme at each width.

---

## 10. Anti-patterns (forbidden)

Do not:

**Colour**

- Use rainbow gradients or bright colours as a background.
- Use bright colours (red, emerald green, bright blue, yellow) for text, icons or status. Banned
  examples: `text-emerald-500`, `bg-green-600`, `text-red-500`, `#27c93f`, `#ff5f56`, `#ffbd2e`.
  Replace them with monochrome symbols (●, ▲, ✕, →).
- Add macOS traffic-light dots (`#ff5f56`, `#ffbd2e`, `#27c93f`) to a code block header.

**Visual**

- Use a border-radius of 16px or more on cards or buttons.
- Use any box-shadow (soft, hard or coloured) on cards, buttons or code blocks. Replace it with
  `border: 1px solid var(--line)`.
- Use 3D icons, emoji as primary icons, or cartoon illustrations.
- Use a full-bleed coloured photo as the hero background.
- Give buttons a gradient or a glow.

**Animation**

- Use heavy bounce, spin or parallax.
- Autoplay video without a `prefers-reduced-motion` check.

**Typography**

- Use script, rounded or decorative display fonts.
- Use weight 700 or more on headings with normal letter-spacing.
- Centre long paragraphs or body text.

**Tooling**

- Use the Tailwind CSS CDN or any utility framework loaded by `<script src="cdn...">`. Write vanilla
  CSS with `:root` variables instead. If you must use Tailwind, use a build step (not a runtime CDN),
  and set colours through CSS variables such as `text-[var(--ink)]`, never `text-emerald-500`.
- Add a framework that injects heavy runtime JavaScript just for styling.

**Structure**

- Replace the theme toggle icons with emoji or text, or rename `#theme-toggle`, `#sun-icon` and
  `#moon-icon`.
- Build a section without a border-top or border-bottom.
- Centre a testimonial with a centred `<blockquote>`.

### Always

- Use a thin 1px border to separate sections.
- Use negative letter-spacing on headings (-0.04em to -0.07em).
- Use UPPERCASE micro-labels with wide tracking (0.15em or more).
- Use SVG line-art illustrations at 1.2px stroke, monochrome.
- Use off-white (#f5f5f5) and off-black (#111). Never pure values.
- Support dark mode, with a working toggle saved to `localStorage`, built from the mandatory markup in
  section 5 (`#theme-toggle`, `#sun-icon`, `#moon-icon`).
- Respect `prefers-reduced-motion`.
- Show a strong `:focus-visible` outline.
- Number the sections (01, 02, 03).
- Put arrows (↗ / ↓) on buttons.
- Keep border-radius at 4px or less on buttons and cards.
- Left-align testimonials in a 2-column grid.

---

## 11. Checklist before delivery

Run through this list before you send the result. Each item is a gate.

**Typography and colour**

- [ ] The palette is monochrome, with no bright colours (red, green, blue, yellow).
- [ ] Headings use Space Grotesk or Inter Tight, weight 500, negative letter-spacing.
- [ ] Body text uses DM Sans or Inter, `color: var(--muted)`, line-height 1.7 or more.
- [ ] No Tailwind colour classes such as `text-emerald-*`, `bg-green-*`, `text-red-*`, and no bright
      hex values.
- [ ] No macOS traffic-light dots in code blocks.

**Layout**

- [ ] Every section has a border-top or border-bottom of 1px `var(--line)`.
- [ ] Each section has an eyebrow micro-label with a number (01, 02, …).
- [ ] The hero uses an asymmetric grid (1.04fr / .96fr, or 7/12 + 5/12).
- [ ] Body text is left-aligned, not centred.

**Components**

- [ ] Buttons have a border-radius of 4px or less and an arrow (↗ or ↓).
- [ ] Cards have no shadow and lift by `translateY(-5px)` on hover.
- [ ] Code blocks have no box-shadow and use a 1px border.
- [ ] The testimonial uses a 2-column grid, left-aligned, not centred.
- [ ] Illustrations are thin-stroke SVG line-art, not photos or emoji.

**Functionality**

- [ ] Dark mode and its toggle work and are saved to `localStorage`.
- [ ] The toggle uses the mandatory markup: `#theme-toggle` with `#sun-icon` and `#moon-icon` inside
      `.header-actions`, `aria-label="Ganti mode gelap/terang"`, `aria-pressed` kept in sync, moon
      visible in light mode and sun visible in dark mode.
- [ ] The mobile nav collapses to a hamburger and closes on Escape and on link click.
- [ ] Reveal on scroll uses `IntersectionObserver`.
- [ ] `prefers-reduced-motion` is respected (all animations off).

**Responsive and touch**

- [ ] `<meta name="viewport">` is present in `<head>`.
- [ ] No horizontal scroll on the page at 320px, 360px, 768px and 1280px.
- [ ] Every grid of 2+ columns collapses to 1 column at 600px (the feature grid may stay at 2 on tablet).
- [ ] Tap targets are at least 44×44px.
- [ ] All hover lifts are wrapped in `@media (hover: hover)`.
- [ ] Code blocks, tables and diagrams scroll inside their own container.
- [ ] Images use `max-width: 100%`.
- [ ] Safe-area insets are applied to the header and footer.

**Accessibility**

- [ ] The skip link exists and works.
- [ ] `:focus-visible` shows a clear 3px outline.
- [ ] Every image has `alt`.
- [ ] Icon-only buttons have `aria-label`.
- [ ] Accordions use native `<details>`/`<summary>`.

**Tooling and structure**

- [ ] No Tailwind CDN or runtime utility framework.
- [ ] Vanilla CSS with `:root` variables. If a framework is used, it has a build step.
- [ ] One complete HTML file (inline `<style>` and `<script>`).
- [ ] The footer is dark, with a copyright bar and a border-top.
- [ ] No border-radius above 4px (except images, at 12–22px).
- [ ] No gradients, soft shadows or glows.

---

## 12. Quick reference (CSS variables)

Every file starts with this block:

```css
:root {
  /* Colour */
  --bg:     #f5f5f5;
  --ink:    #111111;
  --muted:  #666666;
  --line:   #e0e0e0;
  --card:   #eeeeee;
  --accent: #d4d4d4;
  --dark:   #0a0a0a;
  --white:  #ffffff;

  /* Font */
  --font-head: 'Space Grotesk', sans-serif;
  --font-body: 'DM Sans', sans-serif;
  --font-mono: 'JetBrains Mono', monospace;

  /* Radius */
  --radius-sm: 4px;
  --radius-md: 12px;
  --radius-lg: 22px;

  /* Spacing */
  --space-section: clamp(85px, 12vw, 130px);
  --pad-x: clamp(24px, 5vw, 84px);
}

body.dark {
  --bg:     #121212;
  --ink:    #f0f0f0;
  --muted:  #999999;
  --line:   #333333;
  --card:   #1e1e1e;
  --accent: #2a2a2a;
  --dark:   #050505;
  --white:  #181818;
}
```

---

## 13. Style names (for communication)

When the user asks for a variation, use these names so the vocabulary stays consistent:

- **"Swiss-editorial Minimalist"**: the default, described in this file.
- **"Soft Brutalism"**: thicker borders, rougher grid.
- **"Neo-Brutalist Editorial"**: one accent colour and a hard offset shadow. Use only when the user asks
  for it explicitly.
- **"Monochrome Magazine"**: typography-led, no illustrated cards.

Do not describe the style as "modern", "clean" or "sleek". Use the names above.

---

## 14. Example prompt for AI

When a user asks for a new website under this guide, this prompt template works well:

```
Read the SKILLS.md file I attached.

Build a [TYPE: portfolio / landing / company profile] for [NICHE]
called [NAME].

Sections needed:
1. Hero
2. [section 2]
3. [section 3]
...

Specific needs:
- [language]
- [additional section]
- [dark mode default]

Mandatory rules from SKILLS.md (summary):
- Monochrome palette (#f5f5f5 / #111 / #666)
- Space Grotesk headings, weight 500, negative letter-spacing
- 1px var(--line) border as the section divider
- No border-radius above 4px, no shadows
- No bright colours (emerald/red/blue/yellow)
- No Tailwind CDN
- SVG line-art illustrations, not coloured photos
- Working dark mode toggle, using the mandatory sun/moon markup from section 5
- Responsive on mobile: nav → hamburger

Output: one complete HTML file (inline CSS + JS).
```

---

## 15. Version history

- **v3.1**: Added the mandatory theme toggle markup (sun/moon SVG, vanilla CSS, JavaScript with
  `localStorage`) to section 5, with matching entries in the colour rules, accessibility, anti-patterns,
  checklist and example prompt.
- **v3.0**: Rewritten in the structure of the design-taste-frontend guide. Each section now explains why
  its rules hold. Adds a Meta rules section, a delivery checklist with gates, and a clearer anti-pattern
  list. Technical values are unchanged from v2.
- **v2.0**: Fixed CSS syntax. Added `###` subheadings. Banned bright colours, shadows and Tailwind CDN
  outright. Added the testimonial rule, the instruction to decline conflicting requests, and an
  expanded checklist. Added section 14 (example prompt) and section 0 (meta rules).
- **v1.0**: First version.

---

## Summary of changes from v2 to v3

Philosophy unchanged. Technical values unchanged. Only the writing and structure changed: section
rules now state the reason in one line, the checklist items are gates, and frontmatter is added for
the skill's name and trigger description.
