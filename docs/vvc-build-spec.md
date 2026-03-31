# VVC Website Rebuild — Build Spec

> Full visual and structural rebuild of venicevintageclub.com.
> Single `index.html` deployed via GitHub Pages (CNAME: venicevintageclub.com).
> All CSS and JS inline. No framework. No external stylesheets or scripts except Google Fonts.

---

## Brand Context

Venice Vintage Club is a monthly curated vintage pop-up in Venice, CA. "A club, not a store." Community-first, commerce-second. The website should feel like walking into a garage — dark, atmospheric, neon glow, patina, tin signs. NOT a Shopify template. NOT a tech landing page.

- **Founder**: Fred
- **Creative Director**: Morgan
- **First Pop-Up**: May 2, 2026, 6 PM PT, Venice, CA

---

## Design System

### Color Palette (CSS Variables at `:root`)

Extracted from `images/globe-logo-clean.png`:

```css
:root {
  --vvc-void: #0a0a0a;           /* near-black background */
  --vvc-purple: #6B2AF4;         /* primary neon purple (from globe) */
  --vvc-purple-rgb: 107, 42, 244;
  --vvc-amber: #F07A1A;          /* warm orange glow (from globe) */
  --vvc-amber-rgb: 240, 122, 26;
  --vvc-bloom: #E8927C;          /* soft peach/pink */
  --vvc-bloom-rgb: 232, 146, 124;
  --vvc-cream: #F5F0E8;          /* warm off-white text */
  --vvc-cream-rgb: 245, 240, 232;
  --vvc-rust: #8B4513;           /* aged metal / patina accent */
  --vvc-smoke: #1a1a1a;          /* dark gradient start */
  --vvc-smoke-light: #2a2a2a;    /* dark gradient end */
}
```

### Typography

```css
--font-display: 'Bebas Neue', sans-serif;  /* Headlines, signs, gas station feel */
--font-body: 'DM Sans', sans-serif;        /* Body text, clean warm sans */
```

Google Fonts import:
```
https://fonts.googleapis.com/css2?family=Bebas+Neue&family=DM+Sans:ital,wght@0,300;0,400;0,500;0,600;1,400&display=swap
```

### Neon Glow Utility

```css
.neon-glow {
  text-shadow: 0 0 4px var(--vvc-purple), 0 0 11px var(--vvc-purple),
    0 0 22px rgba(var(--vvc-purple-rgb), 0.5), 0 0 40px rgba(var(--vvc-purple-rgb), 0.25);
}
.neon-glow--amber {
  text-shadow: 0 0 4px var(--vvc-amber), 0 0 11px var(--vvc-amber),
    0 0 22px rgba(var(--vvc-amber-rgb), 0.5), 0 0 40px rgba(var(--vvc-amber-rgb), 0.25);
}
```

### Physical Object Treatments

**Tin Sign:**
```css
.tin-sign {
  background: linear-gradient(135deg, #2a2420 0%, #1a1714 100%);
  border: 2px solid var(--vvc-rust);
  border-radius: 3px;
  padding: 16px 28px;
  font-family: var(--font-display);
  color: var(--vvc-cream);
  letter-spacing: 0.08em;
  text-transform: uppercase;
  box-shadow: 4px 6px 20px rgba(0,0,0,0.5), inset 0 1px 0 rgba(255,255,255,0.05);
  transform: rotate(-2deg);
  position: relative;
  overflow: hidden;
}
.tin-sign::after {
  content: '';
  position: absolute;
  inset: 0;
  background: url("data:image/svg+xml,..."); /* inline SVG noise */
  opacity: 0.15;
  mix-blend-mode: overlay;
  pointer-events: none;
}
```

**Letter Board (gas station):**
```css
.letter-board {
  background: #111;
  border: 6px solid #2a2420;
  border-radius: 2px;
  padding: 24px 32px;
  font-family: var(--font-display);
  color: var(--vvc-cream);
  letter-spacing: 0.12em;
  text-transform: uppercase;
  text-align: center;
  box-shadow: 0 0 30px rgba(var(--vvc-amber-rgb), 0.08), 0 8px 30px rgba(0,0,0,0.4);
  /* Horizontal groove lines via repeating-linear-gradient */
  background-image: repeating-linear-gradient(
    0deg, transparent, transparent 28px, rgba(255,255,255,0.03) 28px, rgba(255,255,255,0.03) 30px
  );
}
```

### Atmospheric Effects

- **Film grain**: SVG noise `body::after` overlay at 4% opacity (already exists)
- **Smoky atmosphere**: Per-section radial gradients from `--vvc-void` to `--vvc-smoke`
- **Vignette**: Radial gradient overlay on hero (`transparent center → black edges`)

---

## Scroll Sequence (10 Sections)

### 1. Marquee Banner
- Scrolling ticker (CSS-only `@keyframes` animation)
- Slogans: "May 2nd — First Pop-Up — Venice, CA" / "Join the Club" / "Community Over Everything" / "Racks on Racks on Racks" / "Life Is Short, Shop Vintage" / "Vintage With a Pulse" / "Be First Through the Door"
- Background: `--vvc-void`, border: `--vvc-amber`
- Neon glow on "Join the Club" and "Life Is Short, Shop Vintage"
- Font: `--font-display`, uppercase

### 2. Navigation Bar
- **Left** (community-first): Events | The Space | The Club
- **Center**: VVC logo (`images/vvc-logo-clean.png`, 36x36)
- **Right** (commerce): New Arrivals | Shop | Archive
- All links smooth-scroll to page sections (#events, #space, #about, #drop, #drop, #lookbook)
- Sticky at `top: 55px` (below ticker), transparent → solid on scroll
- Mobile: logo + hamburger only → flat drawer with all 6 links
- Font: `--font-display`, 12px, uppercase, letter-spacing 0.05em
- Industrial styling: no dropdowns, no mega menus

### 3. The Garage Entry (HERO)
- `100vh`, immersive, atmospheric, `overflow: hidden`
- **Background**: `images/hero-placeholder.jpg` with `brightness(0.3) contrast(1.15) saturate(0.7)` — single path swap when real photo arrives
- **3 parallax layers** (JS scroll transforms, disabled on mobile):
  - Layer 1 (bg): garage photo, `translateY(scrollY * 0.3)`
  - Layer 2 (mid): VVC neon sign, `translateY(scrollY * 0.5)`
  - Layer 3 (fg): hanging tin signs, `translateY(scrollY * 0.7)`
- **Neon sign**: `images/vvc-logo-neon.png` positioned at JO'S sign location (~25-30% from top, centered). 3 glow layers (wide blur, close blur, sharp) with flicker keyframes. CSS `filter: drop-shadow()` using `--vvc-purple` and `--vvc-amber`.
- **Hanging tin signs**: 2 signs at slight angles (3-4deg rotation), positioned at top-left and top-right. Slogans: "Vintage With a Pulse" and "Est. 2026".
- **Vignette**: Radial gradient overlay (transparent center → dark edges)
- **Scroll prompt**: "Scroll to explore" + chevron SVG, bounce animation, bottom-center
- `will-change: transform` on parallax layers for GPU compositing

### 4. Full-Bleed Lifestyle Separator
- `images/photo-06.jpg` — Venice beach sunset
- Edge-to-edge, no padding, `height: 70vh`, `object-fit: cover`
- Ken Burns animation: `scale(1) → scale(1.08)` over 20s, infinite alternate
- Purpose: visual breathing room between neon sign (hero) and globe logo (next)

### 5. Globe Logo + Event Section
- `images/globe-logo-clean.png` centered at `clamp(200px, 30vw, 360px)`
- Radial gradient glow behind globe (purple, 15% opacity)
- **Headline**: "May 2nd — Venice, CA" (`--font-display`, large)
- **Subtitle**: "First Pop-Up"
- **Event details**: "Doors at 6 PM. Racks on racks, wall art, collectibles, good music, good people. The first official Venice Vintage Club pop-up."
- **Countdown timer**: Reuse existing JS logic (target: May 2 2026 18:00 PT). Digits in `--font-display`, `--vvc-amber`. Labels in `--font-body`.
- **CTA**: "Get on the List" button → smooth scroll to `#email-capture`
- Min-height `80vh`, centered flex column, `background: var(--vvc-void)`

### 6. The Space (Photo Tour)
- **Photos**: garage-neon.jpg, garage-office.jpg, space.jpg
- **Layout**: CSS Grid bento (`grid-template-columns: repeat(12, 1fr)`, items span different widths). Collapses to single column on mobile.
- **Quote 1**: "A club, not a store." → `.letter-board` treatment (gas station letter board)
- **Quote 2**: "A place where the racks have stories, the walls are for sale, and everyone's invited." → `.tin-sign` treatment (aged metal, noise overlay, slight rotation)
- Photos: `object-fit: cover`, subtle hover scale

### 7. The Drop (Product Preview)
- **Grid**: 3 columns desktop, 2 tablet, 1 mobile
- **4 items**: Vintage Denim Jacket $85, Patina Metal Sign $45, Graphic Band Tee $40, Chevy Remake Sign $60
- **Image placeholder**: Dark card (`--vvc-smoke`) with globe-logo-clean.png watermark at 6% opacity, centered. Text: "Revealed May 2nd" in `--vvc-cream`.
- Each card: item name (h3), price, metadata (size/era/ship or pickup)
- **CTA at bottom**: "Want first pick? DM us on Instagram" → links to instagram.com/venicevintageclub
- When real photos arrive: swap `<img>` src, remove watermark/reveal text

### 8. Lookbook
- **Layout**: CSS columns masonry (`column-count: 3` desktop, 2 tablet, 1 mobile)
- **Curated 10 photos**: photo-04, photo-05, photo-06, photo-10, photo-13, photo-15, photo-17, photo-20, photo-01, photo-03
- **Scroll reveal**: `.fade-in` + `.stagger` classes, Intersection Observer
- **Lightbox**: Click to expand. Fixed fullscreen overlay, `rgba(0,0,0,0.95)`. Close via X button, click outside, or Escape key. Focus trap when open.
- All photos `loading="lazy"`

### 9. The Newsstand
- **Header**: "The Newsstand" / subtitle "Off the Rack"
- **Layout**: Flex row, 4 cards, slight tilt (2-4deg) and overlap
- **Card style**: Magazine cover aesthetic — dark background, thick left border in `--vvc-amber`, aged paper texture via CSS gradients
- **Placeholder content**:
  1. "Founder's Note" — "How a garage in Venice became the club everyone's talking about." (Coming Soon tag)
  2. "May 2nd" — "What to expect, what to wear, what to bring." (Event tag)
  3. "Venice Summer" — "Film photos and fits from the boardwalk." (Lookbook tag)
  4. "The Garage" — "Inside the space where it all started." (Feature tag)
- Hover lifts card and straightens rotation
- HTML structured so swapping content = editing text/links only
- At 768px: horizontal scroll

### 10. Email Capture + Footer
- **Headline**: "Be First Through the Door"
- **Form**: `action="MAILCHIMP_ACTION_URL_PLACEHOLDER"` `method="POST"` `target="_blank"`
- **Fields**: email input + submit button ("Join the Club")
- **Honeypot**: `<div style="position:absolute;left:-5000px" aria-hidden="true"><input name="b_PLACEHOLDER" tabindex="-1" value=""></div>`
- **Success state**: "You're in. We'll let you know." (shown on submit, form hidden)
- Both this section AND the globe/event CTA scroll here
- **Footer**:
  - "Est. 2026, Born 1997"
  - @venicevintageclub → https://instagram.com/venicevintageclub
  - (c) 2026 Venice Vintage Club

---

## Technical Constraints

- **Single file**: Everything in `index.html` — all CSS + JS inline
- **No external deps**: Only Google Fonts CDN
- **Mobile-first**: Base CSS = mobile, `@media (min-width: 768px)` and `(min-width: 1024px)` scale up
- **Lazy loading**: `loading="lazy"` on all images except hero bg + nav logo (`loading="eager"`)
- **Semantic HTML**: heading hierarchy (h1 → h2 → h3), alt text, `aria-label` on sections, keyboard-navigable
- **Keep all images**: No files deleted from /images/
- **GitHub Pages compatible**: No server-side rendering, no build step
- **Lighthouse targets**: Performance 80+, Accessibility 90+
- **Reduced motion**: `@media (prefers-reduced-motion: reduce)` disables parallax, ticker, Ken Burns, flicker, fade-in transitions

## Implementation Phases

1. **Foundation**: HTML structure, CSS vars, typography, nav, responsive shell, skip link
2. **Hero**: Parallax layers, neon sign at JO'S position, tin signs, vignette, scroll-hint
3. **Core Sections**: Separator, globe/event, the space, the drop, lookbook + lightbox, email/footer
4. **Polish**: Newsstand, scroll animations, tin sign treatments, Lighthouse audit, launch.md update

---

## Manual Tasks (Fred — Not Part of Build)

### Before Build
- [ ] Add `hero-placeholder.jpg` to `/images/` (JO'S garage photo)

### Before Going Live
- [ ] Replace `MAILCHIMP_ACTION_URL_PLACEHOLDER` with real Mailchimp form action URL
- [ ] Replace `b_PLACEHOLDER` with real Mailchimp honeypot field name
- [ ] Add Morgan's product photos to `/images/`, update `<img>` src attributes
- [ ] Swap hero-placeholder.jpg for real garage photo
- [ ] Verify @venicevintageclub Instagram handle is live
- [ ] Set up hello@venicevintageclub.com (Google Workspace, $7/mo)
- [ ] Stripe payment links for products (or keep DM-to-purchase for V1)

### Post-May 2nd
- [ ] Swap newsstand placeholder content with real articles
- [ ] Write Founder's Note (anonymous, no byline)
- [ ] Add event recap photos to lookbook
- [ ] Square reader for in-person sales
- [ ] Multi-page routing when inventory scales
