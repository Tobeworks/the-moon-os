# THE MOON RECORDS — SITE LAYOUT SPEC
## Briefing for the HTML Prototype

Stack: Pure HTML + CSS (prototype phase) → Astro + Tailwind CSS (build phase)

---

## TYPOGRAPHY

**Primary:** Cormorant Garamond — Google Fonts
**Data:** DM Mono — Google Fonts

```html
<link rel="preconnect" href="https://fonts.googleapis.com">
<link href="https://fonts.googleapis.com/css2?family=Cormorant+Garamond:ital,wght@0,400;0,500;0,600;1,400;1,500&family=DM+Mono&display=swap" rel="stylesheet">
```

---

## COLOR TOKENS

```css
:root {
  --night-sky:             #080B12;
  --surface:               #0D1018;
  --surface-container-low: #131825;
  --surface-container:     #1A2030;
  --surface-container-high:#242B3A;
  --lunar-silver:          #BCC7D8;
  --on-silver:             #3A4455;
  --on-surface:            #D4DAE4;
  --outline-variant:       #2D3547;
  --glow-silver:           rgba(188, 199, 216, 0.4);
}
```

---

## PAGE STRUCTURE

```
1. HEADER         — fixed, transparent → #0D1018 on scroll
2. HERO           — full bleed, atmospheric gradient, large phase arc
3. CURRENT PHASE  — latest releases (2–3 cards)
4. FIELD          — about the label, brief
5. SUBSCRIBE      — email capture
6. FOOTER
```

---

## 1. HEADER

```
[THE MOON RECORDS]        [RELEASES] [FIELD] [ARCHIVE]     [◑]
```

- Logo left: `THE MOON RECORDS` — Cormorant Garamond SemiBold, `#D4DAE4`, tracking `0.15em`
- Navigation: 3 items — RELEASES / FIELD / ARCHIVE — uppercase, Cormorant Medium, tracking `0.25em`
- Right: live phase indicator `◑` or SVG phase arc in `#BCC7D8` with cold bloom
- `border: none` — separation through background color shift on scroll only
- Fixed position, `backdrop-filter: blur(8px)` when opaque
- Transition: `600ms ease-in-out` on scroll state change

---

## 2. HERO

Full bleed. Atmospheric gradient background. No pixel-grid, no scanlines.

**Background:**
```css
.hero {
  background:
    radial-gradient(
      ellipse 70% 60% at 50% 45%,
      rgba(188, 199, 216, 0.03) 0%,
      transparent 65%
    ),
    #080B12;
  min-height: 100vh;
}
```

**Large Phase Arc — centered, large format:**
```
          ◐
```
SVG moon phase arc, `120px` diameter, `#BCC7D8`, cold bloom filter. Positioned center-top of hero, or slightly offset left with text offset right — decide in prototype.

**Text hierarchy (centered or left-aligned, test both):**

```
AMBIENT / DRONE                     ← DM Mono, 11px, #BCC7D8, tracking 0.4em

The Moon                            ← Cormorant Garamond SemiBold, display scale (5rem+)
Records                             ← same, second line

A label for music that holds still. ← Cormorant Garamond 400 italic, 1.125rem, #D4DAE4, opacity 70%

[CURRENT PHASE]   [ARCHIVE]         ← ghost buttons
```

**Bottom-left — label data (DM Mono, 10px, opacity 40%):**
```
TMR    EST. 2026
FIELD  AMBIENT / DRONE / SOUNDSCAPE
PHASE  —
```

---

## 3. CURRENT PHASE (Latest Releases)

`max-w-4xl`, centered, generous vertical padding.

**Section header:**
```
CURRENT PHASE                       ← Cormorant Garamond SemiBold, uppercase, tracking 0.25em
──────────────────────────────────  ← border-bottom, outline-variant/20
```

**Release cards — horizontal or grid layout (test both):**

```
┌────────────────────────┐
│  [Cover Art 1:1]       │  ← brightness(0.8) saturate(0.6) → full on hover, 600ms
│                        │
├────────────────────────┤
│  Logic Moon            │  ← Cormorant Garamond italic, 13px, #BCC7D8
│  A Field in Winter     │  ← Cormorant Garamond SemiBold, 1.125rem, #D4DAE4
│                        │
│  TMR-001 / FM  38:14   │  ← DM Mono, 10px, opacity 40%
└────────────────────────┘
```

Phase code (`FM`, `WXC`, etc.) visible on every card. Duration in DM Mono.

**Placeholder card if fewer than 3 releases:**
```
┌────────────────────────┐
│         ◑              │  ← phase arc, 32px
│                        │
│  INCOMING              │  ← Cormorant SemiBold, uppercase
│  Next phase pending    │  ← Cormorant italic, opacity 50%
│                        │
│  [SUBSCRIBE]           │
└────────────────────────┘
```

---

## 4. FIELD

2-column layout: label statement left, technical data right.

**Left — Label statement:**

Cormorant Garamond regular, 1rem, line-height 1.8. The label in its own voice — not marketing copy, not bio. The position. 2–3 paragraphs maximum.

**Right — Metadata panel (DM Mono, 11px):**

```
FOUNDED    2026
FORMAT     DIGITAL / LIMITED PHYSICAL
CATALOG    TMR-001 →
SPECTRUM   AMBIENT — DRONE — SOUNDSCAPE
BEATLESS   ALWAYS
```

---

## 5. SUBSCRIBE

Centered, `max-w-2xl`. Minimal.

```
◐                                   ← small phase arc, 24px

Stay in the field.                  ← Cormorant Garamond italic, 1.25rem

Releases and transmissions — nothing else.  ← Cormorant 400, opacity 60%

[your@email.com_____________]  [SUBSCRIBE]
```

Input + button flush, no gap. Ghost button style. No exclamation marks. No urgency.

---

## 6. FOOTER

```
The Moon Records                    RELEASES  FIELD  ARCHIVE

© The Moon Records 2026

TMR  /  Ambient & Drone
```

- `border-top: 1px solid rgba(45, 53, 71, 0.4)` — ghost line
- No status indicators, no live-system language
- Copyright in Cormorant regular, data in DM Mono

---

## GLOBAL CSS COMPONENTS

```css
/* Long-exposure grain overlay */
.grain {
  position: fixed; inset: 0;
  opacity: 0.04;
  pointer-events: none;
  mix-blend-mode: overlay;
  background-image: url('/assets/grain.png'); /* or inline SVG noise */
  z-index: 99;
}

/* Vignette */
.vignette {
  position: fixed; inset: 0;
  background: radial-gradient(
    ellipse at center,
    transparent 35%,
    rgba(8, 11, 18, 0.65) 100%
  );
  pointer-events: none;
  z-index: 98;
}

/* Cold silver bloom — text */
.bloom {
  text-shadow: 0 0 12px rgba(188, 199, 216, 0.4);
}

/* Phase arc container */
.phase-arc {
  filter: drop-shadow(0 0 8px rgba(188, 199, 216, 0.5));
  color: #BCC7D8;
}

/* Ghost button */
.btn-ghost {
  border: 1px solid rgba(188, 199, 216, 0.25);
  color: #BCC7D8;
  background: transparent;
  padding: 0.625rem 1.5rem;
  font-family: 'Cormorant Garamond', serif;
  font-size: 0.75rem;
  letter-spacing: 0.25em;
  text-transform: uppercase;
  transition: border-color 600ms ease-in-out, text-shadow 600ms ease-in-out;
  border-radius: 0;
  cursor: pointer;
}
.btn-ghost:hover {
  border-color: rgba(188, 199, 216, 0.6);
  text-shadow: 0 0 12px rgba(188, 199, 216, 0.4);
}

/* Global transition rule */
* {
  transition-timing-function: ease-in-out;
}
a, button { transition-duration: 600ms; }
```

---

## ASTRO COMPONENT STRUCTURE (build phase)

```
src/
├── layouts/
│   └── BaseLayout.astro       ← grain overlay, vignette, fonts
├── pages/
│   └── index.astro
├── components/
│   ├── Header.astro
│   ├── Hero.astro
│   ├── PhaseArc.astro         ← SVG phase arc, takes illumination % prop
│   ├── CurrentPhase.astro
│   ├── ReleaseCard.astro
│   ├── Field.astro
│   ├── Subscribe.astro
│   └── Footer.astro
└── styles/
    └── global.css             ← grain, vignette, bloom, tokens
```

---

## DESIGN DECISIONS TO TEST IN PROTOTYPE

These are open questions — the HTML prototype resolves them:

1. **Hero text alignment:** centered vs left-offset with phase arc right — which has more atmosphere?
2. **Release card layout:** grid (2–3 columns) vs a single horizontal stack — which reads better for 1–2 releases?
3. **Phase arc in header:** Unicode character (`◐ ◑ ◒ ○ ●`) vs SVG — Unicode is faster to prototype, SVG is more precise
4. **Hero scale:** how large should the display type be before it feels too assertive for this register?
5. **Grain intensity:** 0.03 vs 0.05 — test both, the difference is perceptible

---

## WHAT IS NOT IN THIS SITE

- No blog or news feed
- No social media links in hero (footer only, if at all)
- No video background
- No auto-playing audio
- No loading screen or intro animation
- No chatbot or contact form beyond the email subscribe
- Nothing that announces itself louder than the music would
