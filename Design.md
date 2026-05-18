# Design.md — DLC "The Rent Is Next" Whitepaper Campaign

**Client:** DLC Management Corp.  
**Produced by:** DN Creative LLC / Dan Nemirovsky  
**Campaign:** 2026 Thought Leadership — *"The Rent Is Next"*  
**Launch:** ICSC Las Vegas, May 18–20, 2026  
**Platform:** WordPress (Gutenberg Custom HTML block or Elementor HTML widget)  
**Base file:** `DLC_DataViz_Animated_v2.html` + `DLC_LandingPage_2026_WP_Gutenberg.html`

---

## Quick Navigation

- [Brand Tokens](#brand-tokens) — Colors, shadows, surfaces
- [Typography](#typography) — Type scale, weights, tracking
- [Logo](#logo) — SVG spec, usage rules
- [Page Architecture](#page-architecture) — 8 sections, purpose, content
- [Component Library](#component-library) — Buttons, cards, eyebrows, dividers
- [Data Viz System](#data-viz-system) — 7 archetypes with CSS/JS specs
- [Animation System](#animation-system) — Entry classes, stagger, easing table
- [Responsive Rules](#responsive-rules) — Breakpoints and collapse behavior
- [Chart Design Rules](#chart-design-rules) — Color semantics, labels, sourcing
- [Content Reference](#content-reference) — Headlines, stats, copy per section
- [File Index](#file-index) — All source files

---

## Brand Tokens

### CSS Custom Properties

All colors and surface values are defined as CSS custom properties. Set `data-theme="dark"` on `<html>` — all tokens cascade from there.

```css
/* ── Brand primitives ── */
:root {
  --dlc-blue:  #168FBE;   /* Primary accent — stats, CTAs, key bars, links */
  --dlc-navy:  #213B4E;   /* Primary dark background, hero panels */
  --dlc-green: #5CA547;   /* CTA sections + positive trend indicators only */
  --dlc-teal:  #416464;   /* Supporting surfaces, supply-side chart elements */
  --dlc-grey:  #A3A5A2;   /* Muted UI, secondary text on light, laggard states */
}

/* ── Dark theme (default for this project) ── */
[data-theme="dark"] {
  --bg:             #162736;
  --bg-alt:         #1a303f;
  --surface:        #213B4E;
  --surface-raised: #2a4a60;
  --surface-deep:   #122030;
  --text-primary:   #ffffff;
  --text-secondary: rgba(255,255,255,0.75);
  --text-muted:     rgba(255,255,255,0.45);
  --text-faint:     rgba(255,255,255,0.22);
  --border:         rgba(255,255,255,0.08);
  --border-accent:  rgba(22,143,190,0.35);
  --accent:         #168FBE;
  --accent-dim:     rgba(22,143,190,0.12);
  --accent-mid:     rgba(22,143,190,0.28);
  --green:          #5CA547;
  --teal:           #416464;
  --bar-passive:    rgba(255,255,255,0.14);
  --shadow:         0 12px 48px rgba(0,0,0,0.55);
  --shadow-sm:      0 2px 16px rgba(0,0,0,0.4);
  --shadow-blue:    0 8px 32px rgba(22,143,190,0.18);
  --grid-dot:       rgba(255,255,255,0.04);
}

/* ── Light theme (optional toggle) ── */
[data-theme="light"] {
  --bg:             #f0f4f7;
  --surface:        #ffffff;
  --surface-raised: #ffffff;
  --surface-deep:   #eaf0f5;
  --text-primary:   #213B4E;
  --text-secondary: rgba(33,59,78,0.72);
  --text-muted:     rgba(33,59,78,0.45);
  --text-faint:     rgba(33,59,78,0.20);
  --border:         rgba(33,59,78,0.10);
  --border-accent:  rgba(22,143,190,0.28);
  --accent:         #168FBE;
  --accent-dim:     rgba(22,143,190,0.08);
  --accent-mid:     rgba(22,143,190,0.20);
  --bar-passive:    rgba(33,59,78,0.12);
  --shadow:         0 12px 48px rgba(33,59,78,0.12);
  --shadow-blue:    0 8px 32px rgba(22,143,190,0.10);
  --grid-dot:       rgba(33,59,78,0.04);
}
```

### Background Dot-Grid Texture

```css
body {
  background-image: radial-gradient(var(--grid-dot) 1px, transparent 1px);
  background-size: 28px 28px;
}
```

### Color Discipline Rules

- **DLC Blue `#168FBE` is the only accent color.** One focal point per surface.
- **DLC Green `#5CA547`** appears only on CTA sections and positive trend arrows. Never on charts.
- **DLC Teal `#416464`** and Navy variants do all surface work — they are not accents.
- Never use off-brand variants: `#004E66`, `#0A82C8`, `#3DAA35` — wrong. Use the official hex values above.
- DLC Navy `#213B4E` is the correct dark background, not `#0A1620`.

---

## Typography

### Primary: Soleil (Adobe Fonts)

DLC's corporate typeface. Required for all branded deliverables. The DLC logotype is derived from Soleil Bold with modified letter widths.

**Required weights:** 400 Regular · 600 Semibold · 700 Bold · 800–900 Extrabold

**Web font delivery (WordPress):**
```
Upload .woff2/.woff to: /wp-content/themes/[theme]/fonts/
Register via add_action('wp_head', ...) in functions.php
```

### Fallback: Lato (Google Fonts)

Active fallback for all coded deliverables until Soleil is available.

```html
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Lato:wght@300;400;700;900&display=swap" rel="stylesheet">
```

**Required weights:** 300 Light · 400 Regular · 700 Bold · 900 Black

### Font Stack

```css
font-family: 'Soleil', 'Lato', Arial, sans-serif;
```

### No Serif Fonts — Ever

Georgia, Times New Roman, or any serif typeface is not part of the DLC brand system. All DLC work uses Soleil (or Lato). This is not negotiable.

### Type Scale

| Role | Size | Weight | Tracking | Usage |
|---|---|---|---|---|
| Display / Hero H1 | `clamp(42px, 6vw, 80px)` | 900 | -0.5 to -3px | Hero headline, closing anchor statement |
| Section H2 | `clamp(28px, 3.8vw, 52px)` | 900 | -0.5 to -2px | Section headings |
| Lead / Subhead | `clamp(16px, 1.5vw, 20px)` | 300 | 0 | Subheads, intro copy |
| Body | `clamp(15px, 1.2vw, 17px)` | 300–400 | 0 | Paragraph body, 1.7–1.8 line-height |
| Eyebrow / Kicker | `11px` | 700 | 2–4px | ALL CAPS, section labels, source tags |
| Stat Numeric | `clamp(64px, 8vw, 112px)` | 900 | -3px | Count-up stat values |
| Pull Quote | `clamp(18px, 2vw, 24px)` | 300 italic | 0 | Quote blocks |
| Chart Number (hero) | `112px` | 900 | -0.02em | Archetype 01 hero stat |
| Chart Number (standard) | `66px` | 900 | -0.02em | Archetype 02 three-stat row |

### Font Weight Map (Lato)

| Weight | Usage |
|---|---|
| 300 | Body copy, chart subtitles, source labels, pull quotes |
| 400 | UI labels, secondary content |
| 700 | Eyebrows, kickers, badges, uppercase labels |
| 900 | Stat numerics, headlines, all large display text |

### Spacing System

| Token | Value | Usage |
|---|---|---|
| `--section-pad` | `clamp(80px, 10vw, 128px)` | Vertical section padding |
| `--gutter` | `clamp(24px, 5vw, 80px)` | Horizontal section padding |
| `--col-max` | `1160px` | Container max-width |
| Card padding | 36–40px | Cycle cards, data blocks |
| Stat block padding | 60 × 40px | Stat rows |

---

## Logo

### Specification

The DLC logo SVG (`dlc-white-logo.svg`) is 200×55px, all white fills. The symbol (left) is a three-part triangular shape; the logotype (right) reads "DLC" in Soleil Bold-derived letterforms.

```
File:     dlc-white-logo.svg
Location: C:\Users\vikin\OneDrive\Desktop\DN Creative\DLC Whitepaper\dlc-white-logo.svg
Viewbox:  0 0 200 55
Colors:   All fill="#FFFFFF" — for dark backgrounds only
```

### Usage Rules

- **Always embed inline** for web builds — never use `<img src>` for the logo.
- **Symbol + logotype must always appear together** — never the logotype alone.
- Use the white SVG for all dark-background designs (this entire project).
- Full-color version: Blue top, Green diagonal, Teal lower for light backgrounds.
- Never use raster versions, text abbreviations, or placeholder treatments.

### Color Modes Reference

| Background | Treatment |
|---|---|
| Dark (this project) | White SVG — `dlc-white-logo.svg` |
| Light | Full color symbol + dark logotype |
| Monotone | DLC Blue `#168FBE` only |
| Single color | 100% black |

---

## Page Architecture

Single-page narrative scroll experience. Dark theme, editorial/declarative. Target desktop height: 6,500–7,500px.

| # | Section | Purpose | Key Content |
|---|---|---|---|
| 00 | WP Header/Nav | Theme chrome | Wrap: `<!-- WP_CHROME_PLACEHOLDER_START/END -->` |
| 01 | Hero | Cover statement | "The Rent Is Next." · "Fundamentals Moved First, Pricing Is Now Following" |
| 02 | CEO Letter | Authority signal | Adam Ifshin editorial letter. Two-column: text left, headshot right. |
| 03 | The Pressure Has Been Built | Structural imbalance argument | Demand vs. Supply split panel. 4.8% availability · 11.3 MSF absorption · 5.0 MSF completions · $24.34 asking rent |
| 04 | The Release | How pricing moves | Then/Now equation + Net Effective Rent bar chart |
| 05 | The Age of the Operator | Competitive implication | 4 pillars · Winners/Laggards · "Waiting Is Expensive" anchor |
| 06 | The Proof | Evidence mode | Boot Barn case study · +19.1% net sales · 65–70 planned openings · ~3.3% occupancy cost · +25% sales per $1 rent since 2019 |
| 07 | Closing Statement | Typographic anchor | "If you are reacting to it, you are already behind." No imagery. |
| 08 | CTA / Download | Conversion | Download full report · Conference registration (ICSC 2026) |
| 09 | WP Footer | Theme chrome | Wrap in same markers as 00 |

### Hero (Section 01) — Visual Spec

- Full-bleed photography — open-air retail center, low-angle, blue sky
- DLC Navy `#213B4E` overlay at 70–80% opacity
- Eyebrow: `THE RENT IS NEXT` — 11px, DLC Blue, 4px tracking, ALL CAPS
- H1: `"Fundamentals Moved First, Pricing Is Now Following"` — Soleil 900, `clamp(42px, 6vw, 80px)`, key word in DLC Blue italic
- Subhead: Lead copy, Lato 300, max-width 560px
- DLC white SVG logo visible in fixed nav
- Scroll progress bar: 3px, fixed top, DLC Blue with glow

### Closing Statement (Section 07) — Visual Spec

- Pure DLC Navy or deep `#162736` background. Zero imagery.
- Single anchor line at maximum scale: Soleil 900, `clamp(52px, 7vw, 96px)`
- Silence around the line IS the design — generous vertical padding
- DLC Blue italic on the key emphasis word

---

## Component Library

### Primary CTA Button

```css
.btn-primary {
  background: #168FBE;
  color: white;
  font-size: 15px; font-weight: 700; letter-spacing: 2px; text-transform: uppercase;
  padding: 20px 52px; border-radius: 4px;
  box-shadow: 0 12px 32px rgba(22,143,190,0.35);
  transition: background 0.25s, transform 0.25s, box-shadow 0.25s;
}
.btn-primary:hover {
  background: #1aa8e0;
  transform: translateY(-3px);
  box-shadow: 0 18px 40px rgba(22,143,190,0.45);
}
```

### Ghost Button

```css
.btn-ghost {
  color: rgba(255,255,255,0.75);
  border-bottom: 1px solid rgba(255,255,255,0.2);
  padding: 16px 0;
  transition: color 0.2s, border-color 0.2s;
}
.btn-ghost:hover { color: white; border-color: white; }
```

### Eyebrow / Kicker

```css
.eyebrow {
  font-size: 11px; font-weight: 700; letter-spacing: 3px;
  text-transform: uppercase; color: var(--accent); margin-bottom: 14px;
}
```

### Pull Quote Block

```css
.pull-quote {
  border-left: 3px solid var(--accent);
  padding: 24px 28px;
  background: rgba(22,143,190,0.06);
  border-radius: 0 6px 6px 0;
  font-size: clamp(18px, 2vw, 24px);
  font-weight: 300; font-style: italic; color: white;
}
```

### Stat Block

```css
.stat-block {
  background: rgba(22,39,54,0.85);
  padding: 60px 40px;
}
.stat-block .number {
  font-size: clamp(64px, 8vw, 112px); font-weight: 900;
  color: var(--accent); letter-spacing: -3px;
  font-variant-numeric: tabular-nums;
}
.stat-block .label {
  font-size: 14px; font-weight: 300;
  color: var(--text-muted); max-width: 180px;
}
```

### Cycle / Pillar Card

```css
.pillar-card {
  background: rgba(255,255,255,0.04);
  border: 1px solid rgba(255,255,255,0.07);
  border-radius: 8px; padding: 40px 36px;
  transition: background 0.3s, border-color 0.3s, transform 0.3s cubic-bezier(0.22,1,0.36,1);
}
/* Top accent rule: scaleX(0) → scaleX(1) on hover */
.pillar-card::before {
  content: ''; display: block; height: 2px;
  background: var(--accent); transform: scaleX(0); transform-origin: left;
  transition: transform 0.35s cubic-bezier(0.22,1,0.36,1);
  margin-bottom: 20px;
}
.pillar-card:hover { background: rgba(22,143,190,0.07); border-color: rgba(22,143,190,0.2); transform: translateY(-4px); }
.pillar-card:hover::before { transform: scaleX(1); }
```

### Data Block (Proof Section)

```css
.data-block {
  background: rgba(22,39,54,0.95); padding: 48px 40px;
}
.data-block h3 { font-size: 22px; font-weight: 900; color: white; }
.data-block li::before { content: ''; display: inline-block; width: 6px; height: 6px; border-radius: 50%; background: var(--accent); margin-right: 10px; }
.data-block .source-strip { font-size: 11px; color: rgba(255,255,255,0.3); letter-spacing: 0.5px; }
```

### Form Field

```css
.form-field {
  background: rgba(255,255,255,0.05);
  border: 1px solid rgba(255,255,255,0.1);
  padding: 14px 18px; border-radius: 4px;
  color: white; font-size: 15px;
}
.form-field::placeholder { color: rgba(255,255,255,0.25); }
.form-field:focus { border-color: var(--accent); background: rgba(22,143,190,0.05); outline: none; }
.form-label { font-size: 11px; font-weight: 700; letter-spacing: 2px; text-transform: uppercase; color: rgba(255,255,255,0.45); }
```

### Section Transition Divider

```css
.section-divider {
  height: 2px;
  background: linear-gradient(90deg, transparent, var(--dlc-blue), transparent);
  opacity: 0.25;
}
```

### Scroll Progress Bar

```css
.scroll-progress {
  position: fixed; top: 0; left: 0;
  height: 3px; width: 0%;
  background: var(--dlc-blue);
  box-shadow: 0 0 10px rgba(22,143,190,0.7);
  z-index: 10000; transition: width 0.1s linear;
}
```

```javascript
window.addEventListener('scroll', () => {
  const scrollTop    = document.documentElement.scrollTop;
  const scrollHeight = document.documentElement.scrollHeight - document.documentElement.clientHeight;
  document.getElementById('scrollProgress').style.width = (scrollTop / scrollHeight) * 100 + '%';
}, { passive: true });
```

---

## Data Viz System

### Chart Card Base

Every archetype wraps in `.chart-card`. This handles the hover glow, left accent rule, and layout foundation. Never skip this wrapper.

```css
.chart-card {
  background: var(--surface);
  border-radius: 8px; overflow: hidden; margin-bottom: 12px;
  box-shadow: var(--shadow-sm); border: 1px solid var(--border);
  transition: background 0.45s ease, border-color 0.45s ease,
              box-shadow 0.3s ease, transform 0.3s cubic-bezier(0.22,1,0.36,1);
  position: relative;
}
.chart-card::before {
  content: ''; position: absolute; left: 0; top: 0; bottom: 0; width: 3px;
  background: var(--accent); transform: scaleY(0.4); transform-origin: center;
  opacity: 0; transition: transform 0.4s cubic-bezier(0.22,1,0.36,1), opacity 0.3s ease;
}
.chart-card:hover::before { transform: scaleY(1); opacity: 1; }
.chart-card:hover {
  border-color: var(--border-accent);
  box-shadow: var(--shadow), var(--shadow-blue);
  transform: translateY(-2px);
}
```

### Archetype Summary

| # | Name | `data-anim` | Use Case | Key Stat |
|---|---|---|---|---|
| 01 | Hero Number | `hero` | Single commanding stat that leads a section | 94% / any large single value |
| 02 | Three-Stat Row | `threestats` | Three supporting stats side-by-side after a hero | 4.8% · 11.3 MSF · $24.34 |
| 03 | Bar Chart | `bars` | Time-series bars, one highlighted current-period bar | Net absorption trend, 2014–2026 |
| 04 | Line Chart | `line` | Cap rate / rent / occupancy trend over time | Asking rent trend |
| 05 | Split Stat Panel | `split` | Large stat left, explanation copy right | 60% / any stat needing context |
| 06 | Comparison Reframe | `compare` | Before/after or then/now with conclusion beneath | 6.5% → 4.8% availability |
| 07 | Donut / Proportion | `donut` | Single percentage as circular arc. Once per section max. | 78% physical occupancy |

### Archetype 01 — Hero Number

**Padding:** 56px × 60px  
**Number size:** 112px, weight 900, DLC Blue, glow: `0 0 60px rgba(22,143,190,0.3)`  
**Unit size:** 48px, weight 900, `var(--text-primary)`, `margin-top: 16px`  
**Layout:** Two-column grid — `auto 1fr`, gap 56px  
**Count-up:** Fires immediately on intersection, no delay

```html
<div class="chart-card" data-anim="hero">
  <div class="hero-wrap">
    <div>
      <div class="hero-kicker anim-fade-up">EYEBROW LABEL</div>
      <div class="hero-number anim-fade-up delay-1">
        <span class="count-up" data-target="94" data-decimals="0">0</span>
        <span class="unit">%</span>
      </div>
      <div class="hero-num-label anim-fade-up delay-2">Descriptor</div>
    </div>
    <div>
      <div class="hero-headline anim-fade-up delay-1">YOUR HEADLINE</div>
      <p class="hero-body anim-fade-up delay-2">Supporting body copy. Lato 300. 2–3 sentences.</p>
      <div class="hero-source anim-fade-up delay-3">Source: Your Source</div>
    </div>
  </div>
</div>
```

### Archetype 02 — Three-Stat Row

**Padding:** 44px × 48px  
**Number size:** 66px, weight 900, DLC Blue, glow: `0 0 40px rgba(22,143,190,0.2)`  
**Layout:** `grid-template-columns: 1fr 1px 1fr 1px 1fr` — the `1px` column IS the divider  
**Kicker rule:** Draws left-to-right via `scaleX(0) → scaleX(1)` when `.is-visible` added  
**Count-up delay:** 100ms

Live content: `4.8%` Availability Rate · `11.3 MSF` Net Absorption · `$24.34` Asking Rent

### Archetype 03 — Bar Chart

**SVG viewBox:** `0 0 960 240`  
**Bar animation:** `scaleY(0) → scaleY(1)` per `.bar-rect` when `.is-bars-animated` added to card  
**Easing:** `cubic-bezier(0.34, 1.28, 0.64, 1)` — spring overshoot for bars  
**Stagger:** 0.05s per bar (`nth-child` selectors)  
**Value labels:** Fade in at `0.9s` after animation start  
**Critical:** `transform-box: fill-box; transform-origin: bottom` required on every `.bar-rect`  
**Color map:** DLC Blue for current/key bar · `rgba(255,255,255,0.14)` for passive/historical

### Archetype 04 — Line Chart

**SVG viewBox:** `0 0 960 230`  
**Three simultaneous animations:**
1. Line draw: `stroke-dashoffset` from `getTotalLength()` → 0 over 1.6s (Material decelerate easing)
2. Area fill: `opacity 0 → 1` over 1.2s, 0.3s delay
3. Dot pop: `scale(0) → scale(1)`, staggered from 0.40s to 1.52s, spring easing

**Critical:** Must call `getBoundingClientRect()` between setting `strokeDashoffset` and setting the transition to force reflow.  
**Main line:** Must have `id="mainLine"` — the JS animator looks for this ID.

### Archetype 05 — Split Stat Panel

**Layout:** `grid-template-columns: 280px 1fr`, min-height 280px  
**Left panel:** `background: var(--accent-dim)`, `border-right: 1px solid var(--border-accent)` — number side  
**Right panel:** Copy, claim headline, trend indicator  
**Number size:** 92px, DLC Blue, glow: `0 0 60px rgba(22,143,190,0.35)`  
**Right panel animation:** `anim-fade-right` with stagger delays  
**Trend arrows:** `↑` uses `color: var(--green)` (DLC Green `#5CA547`), weight 900

### Archetype 06 — Comparison Reframe

**Layout:** `grid-template-columns: 1fr 64px 1fr`  
**Dim side (left):** Muted — `color: var(--text-muted)` on number and label  
**Bright side (right):** DLC Blue — `color: var(--accent)` + glow  
**Reframe line:** Fades in at `0.7s` delay (after count-ups near completion), uppercase, 22px weight 900  
**VS divider:** 12px, weight 900, `var(--text-faint)`, ALL CAPS  
**Count-up delay:** 100ms

Live content: `6.5%` (then) vs `4.8%` (now) — availability rate

### Archetype 07 — Donut / Proportion

**Layout:** `grid-template-columns: 230px 1fr`, gap 52px  
**SVG:** Circle at `cx=95 cy=95 r=57`, `stroke-width=22`  
**Circumference:** `2 × π × 57 ≈ 358`  
**Dashoffset formula:** `358 × (1 - percentage_as_decimal)`

| Target % | `stroke-dashoffset` |
|---|---|
| 90% | 36 |
| 78% | 79 |
| 65% | 125 |
| 50% | 179 |

**Arc start:** `transform="rotate(-90 95 95)"` — rotates start to top of circle  
**Arc glow:** `filter: drop-shadow(0 0 6px rgba(22,143,190,0.5))`  
**Count-up and arc draw:** Start simultaneously at 150ms after card enters viewport  
**Max donut size:** 200px. Never enlarge — center text layout breaks.  
**Frequency:** One donut per scroll section maximum.

---

## Animation System

### CSS Entry Classes

All animated elements start invisible and translate into position when `.is-visible` is added by the IntersectionObserver.

```css
.anim-fade-up   { opacity: 0; transform: translateY(28px); transition: opacity 0.7s cubic-bezier(0.22,1,0.36,1), transform 0.7s cubic-bezier(0.22,1,0.36,1); }
.anim-fade-left { opacity: 0; transform: translateX(-36px); transition: opacity 0.65s cubic-bezier(0.22,1,0.36,1), transform 0.65s cubic-bezier(0.22,1,0.36,1); }
.anim-fade-right{ opacity: 0; transform: translateX(36px);  transition: opacity 0.65s cubic-bezier(0.22,1,0.36,1), transform 0.65s cubic-bezier(0.22,1,0.36,1); }
.anim-scale     { opacity: 0; transform: scale(0.92);       transition: opacity 0.6s cubic-bezier(0.22,1,0.36,1), transform 0.6s cubic-bezier(0.22,1,0.36,1); }

.is-visible.anim-fade-up,
.is-visible.anim-fade-left,
.is-visible.anim-fade-right,
.is-visible.anim-scale { opacity: 1; transform: none; }
```

### Stagger Delays

```css
.delay-1 { transition-delay: 0.10s !important; }
.delay-2 { transition-delay: 0.22s !important; }
.delay-3 { transition-delay: 0.34s !important; }
.delay-4 { transition-delay: 0.46s !important; }
.delay-5 { transition-delay: 0.58s !important; }
```

### Chart State Classes (IntersectionObserver → JS)

| `data-anim` | State class added | Triggers |
|---|---|---|
| `hero` | — | Count-up immediately |
| `threestats` | — | Count-up at 100ms |
| `bars` | `is-bars-animated` | Bar scaleY at 200ms |
| `line` | `is-line-animated` | Line draw at 300ms |
| `split` | — | Count-up at 150ms |
| `compare` | `is-compare-animated` | Count-up + reframe line at 100ms |
| `donut` | `is-donut-animated` | Arc draw + count-up at 150ms |

All cards fire **once only** — `cardObserver.unobserve(card)` after trigger.

### IntersectionObserver Config

```javascript
const observerOpts = { threshold: 0.18, rootMargin: '0px 0px -40px 0px' };
```

### Count-Up Animation

```javascript
// Duration: 1800ms. Easing: easeOutQuart (starts fast, decelerates to target)
function easeOutQuart(t) { return 1 - Math.pow(1 - t, 4); }
```

HTML usage: `<span class="count-up" data-target="94" data-decimals="0">0</span>`

### Full Easing Reference

| Animation | Duration | Easing |
|---|---|---|
| Fade up / left / right | 0.65–0.70s | `cubic-bezier(0.22, 1, 0.36, 1)` |
| Scale in (donut) | 0.60s | `cubic-bezier(0.22, 1, 0.36, 1)` |
| Kicker rule draw | 0.90s | `cubic-bezier(0.22, 1, 0.36, 1)` |
| Bar spring | 0.75s | `cubic-bezier(0.34, 1.28, 0.64, 1)` — slight overshoot |
| Bar value label | 0.40s at 0.9s delay | `ease` |
| Line path draw | 1.60s | `cubic-bezier(0.4, 0, 0.2, 1)` |
| Area fill | 1.20s at 0.3s delay | `ease` |
| Dot pop | 0.40s, staggered | `cubic-bezier(0.34, 1.56, 0.64, 1)` — stronger spring |
| Annotation box | 0.50s at 1.6s delay | `ease` |
| Donut arc | 1.60s at 0.2s delay | `cubic-bezier(0.4, 0, 0.2, 1)` |
| Reframe conclusion | 0.60s at 0.7s delay | `cubic-bezier(0.22,1,0.36,1)` |
| Count-up | 1800ms | `easeOutQuart` (JS) |
| Scroll progress | 0.10s | `linear` |
| Card hover lift | 0.30s | `cubic-bezier(0.22, 1, 0.36, 1)` |
| Theme transition | 0.45s | `ease` |

---

## Responsive Rules

```css
@media (max-width: 900px) {
  /* All multi-column grids collapse to single column */
  .hero-wrap          { grid-template-columns: 1fr; gap: 28px; padding: 40px 32px; }
  .three-grid         { grid-template-columns: 1fr; gap: 24px; }
  .stat-divider-v     { display: none; }
  .stat-col           { padding: 0; }
  .split-grid         { grid-template-columns: 1fr; }
  .compare-cols       { grid-template-columns: 1fr; gap: 20px; }
  .vs-divider         { text-align: left; }
  .donut-layout       { grid-template-columns: 1fr; }
  /* CEO letter: text + headshot stack vertically. Headshot caps at 320px. */
  /* Form: collapses to 1 column */
}

@media (max-width: 600px) {
  /* Type caps */
  .header-title  { font-size: 28px; }
  .hero-number   { font-size: 68px; }
  .split-big-num { font-size: 72px; }
  .big           { font-size: 56px; }
  /* Hero H1 caps at 36px */
  /* Stat values cap at 72px */
  /* CTA buttons go full-width */
}
```

---

## Chart Design Rules

**1. One Color = One Meaning**  
`#168FBE` (DLC Blue) = current cycle, key data, primary insight. White/grey = historical, context, secondary. `#5CA547` (DLC Green) = positive trend arrows only. Never use DLC Blue for two different things on the same chart.

**2. Gridlines Disappear**  
Gridlines at White 5.5% opacity on dark / Navy 6% on light. If you can clearly read them, they are too strong. The data is the story.

**3. Labels Are Data, Not Decoration**  
Every label earns its place. Remove any label that does not change the reader's understanding. Axis labels on bars are optional if heights make values obvious. Value callouts appear only on the key bar/data point.

**4. Numbers Drive the Layout**  
Build card dimensions from the number outward — not the other way. A 112px stat needs breathing room. Never crush a large number into a tight container.

**5. Source Strip Is Required**  
Every chart that shows data has a source row. Format: `CBRE Research · CoStar · JLL Retail · Green Street · Placer.ai` (use only sources that apply). Source: 11px, `rgba(255,255,255,0.28)`, tracking 0.5px.

**6. One Donut Per Section Maximum**  
The donut is the most visually heavy archetype. Use once per scroll section. Never two donuts in the same card.

---

## Content Reference

### Key Statistics (from live site)

| Stat | Value | Context |
|---|---|---|
| Availability Rate | **4.8%** | Section 03 — The Pressure |
| Net Absorption | **11.3 MSF** | Section 03 |
| Completions | **5.0 MSF** | Section 03 |
| Asking Rent | **$24.34** | Section 03 |
| Retail Sales PSF | **+4.7%** | Section 04 — Market Fundamentals |
| Asking Rents PSF | **+3.7%** | Section 04 |
| Retail Sq Ft Per Person | **−1.6%** | Section 04 |
| Sales per $1 Rent since 2019 | **+25%** | Section 04 |
| Boot Barn Net Sales Growth | **+19.1%** | Section 06 — The Proof |
| Boot Barn Planned Openings | **65–70** | Section 06 |
| Boot Barn Store Count Potential | **2×** | Section 06 |
| Boot Barn Occupancy Cost Ratio | **~3.3%** | Section 06 |

### Key Headlines

| Section | Headline |
|---|---|
| Hero | "THE RENT IS NEXT" |
| Hero sub | "Fundamentals Moved First, Pricing Is Now Following" |
| CEO letter intro | "Pricing in retail real estate is beginning to reflect the fundamentals of the market" |
| Closing anchor | "If you are reacting to it, you are already behind." |

### Media / Credibility Logos

Displayed in the Proof section: New York Times · CBRE · CoStar · Cushman & Wakefield · CNBC · Newmark · NPR · Realtor.com · USA Today

### Sources to Cite

`CBRE Research · CoStar · JLL Retail · Green Street · Placer.ai`

---

## Image Token System

Use these exact tokens in HTML — swap via find-and-replace, no rebuild needed.

| Token | Section | Description |
|---|---|---|
| `{{IMG_HERO_STOREFRONT}}` | 01 Hero | Open-air retail center, low-angle, blue sky, 16:9 |
| `{{IMG_ADAM_HEADSHOT}}` | 02 CEO Letter | Adam Ifshin editorial portrait, 3:4, blue blazer |
| `{{IMG_AERIAL_CENTER}}` | 03 Pressure | Aerial retail center with parking lot, 16:9 |
| `{{IMG_STOREFRONT_THEN}}` | 04 Release | Muted/desaturated storefront, 1:1 |
| `{{IMG_STOREFRONT_NOW}}` | 04 Release | Active modern storefront, DLC Blue tint, 1:1 |
| `{{IMG_NORDSTROM_RACK}}` | 05 Operator | Nordstrom Rack exterior, parking lot, blue sky, 16:9 |
| `{{IMG_PILLAR_1}}` – `{{IMG_PILLAR_4}}` | 05 Pillars | Retail center card photos, 4:5 each |
| `{{IMG_DLC_LOGO_WHITE}}` | All sections | `dlc-white-logo.svg` — always inline SVG, never raster |

---

## Open TBD Items

Mark all TBDs in HTML with `<!-- TBD: brief-§N-item-N -->` for searchability.

1. Net effective rent values — Section 04, Moment 4.3 (owner: Adam/Chris)
2. DLC portfolio stat numbers — Section 06, Moment 6.3 (owner: DLC analytics)
3. Hero photo final selection — DLC portfolio vs. licensed stock (owner: Katrina/Dan)
4. Adam Ifshin headshot — high-res production version (owner: Katrina/DLC)
5. Form integration — Hubspot vs. WP-native (owner: Birdhouse/DLC)
6. Optional pull quote — Section 06 CBRE/JLL commentary (owner: Katrina)
7. Working title "The Rent Is Next" — confirm with Adam (owner: Katrina)
8. Form wire-up — production pass (owner: Birdhouse)

---

## WordPress Integration

### Embed Options

**Option A — Full page (recommended):**  
Blank / No Header/Footer page template. Paste full HTML into Custom HTML block or directly into template PHP.

**Option B — Elementor:**  
Full-Width page. One HTML widget per chart section. CSS in Elementor's Custom CSS panel. JS in a single HTML widget before `</body>`.

**Option C — Gutenberg Custom HTML:**  
Individual chart sections per block. Shared CSS/JS via `wp_enqueue_style` / `wp_enqueue_scripts` in `functions.php`.

### Preventing Flash of Unstyled Content

```html
<html lang="en" data-theme="dark">
```

Set this directly in `header.php` or inline in the page template.

### Font Delivery (Soleil on WordPress)

```
1. Upload Soleil-Bold.woff2 / .woff to: /wp-content/themes/[theme]/fonts/
2. Register in functions.php:
   add_action('wp_head', function() {
     echo '<style>
       @font-face { font-family: "Soleil";
         src: url("'.get_template_directory_uri().'/fonts/Soleil-Bold.woff2") format("woff2");
         font-weight: 700; font-display: swap; }
     </style>';
   });
```

Soleil swap: single find-and-replace `'Lato', sans-serif` → `'Soleil', 'Lato', sans-serif` across all declarations.

---

## File Index

| File | Purpose |
|---|---|
| `DLC_DataViz_Animated_v2.html` | Primary data viz source — 7 animated archetypes, self-contained |
| `DLC_DataViz_Dev_Handoff.md` | Full developer spec for data viz system (this document's viz source) |
| `DLC_LandingPage_2026_WP_Gutenberg.html` | CSS architecture + scroll/animation engine — always the base |
| `DLC_LandingPage_2026_BuildBrief.md` | Master landing page build brief |
| `DLC_DataViz_Reference.html` | Static data viz reference for Style B |
| `DLC_Canva_Deck_Outline.html` | v7 content outline — section copy, hero lines |
| `DLC_LandingPage_2026_Wireframe.html` | Pre-build structural wireframe |
| `DLC_LandingPage_2026_WP_CustomCSS.css` | Standalone CSS for WP theme injection |
| `DLC_Whitepaper_Strategy_2026_UPDATED.html` | Full strategy blueprint |
| `dlc-white-logo.svg` | Official DLC white SVG logo — embed inline, never raster |
| `Design.md` | This file — single design reference for the project |

### External References

| Reference | URL / Path |
|---|---|
| Live page reference | https://www.dlcmgmt.com/therentisnext/ |
| Prior campaign baseline | https://dlcmgmt.com/too-good-to-ignore |
| Figma project | https://www.figma.com/design/qcvjmeN3HIQwqwbNxKbNSf/ |
| Brand guidelines source | DLC Branding Guidelines.pdf (September 2023) |

### Key Contacts

| Person | Role |
|---|---|
| Katrina Mullaney | Content/project manager — primary day-to-day contact |
| Adam Ifshin | CEO, DLC — must approve copy before design is finalized |
| Chris | Content reviewer — four-section structure confirmed |
| Birdie / Birdhouse | Development — receives phased Figma handoff, builds landing page. Min. 2-week build window. |

---

*DLC Management Corp. — 2026 Thought Leadership Campaign*  
*DN Creative LLC / Dan Nemirovsky · May 2026*  
*Single design reference for all deliverables in this project directory.*
