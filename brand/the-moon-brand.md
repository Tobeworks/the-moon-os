# THE MOON RECORDS — Brand Reference Document
## Living Document

---

## 1. Creative Direction

**North Star: "Lunar Program"**

The Moon Records visual system references the graphic language of the space program era (1960s–1970s) filtered through Bauhaus discipline. Not nostalgic reproduction — a specific aesthetic position: geometric precision, functional form, instrument-grade clarity. The visual grammar of a mission document, not a mood board.

This is not atmospheric. It does not evoke. It states.

The Bauhaus principle is the foundation: form follows function. Every element on a Moon Records asset has a structural reason to exist. Decoration without function is a violation. The design reads like a technical schematic for something that has never been built — or like the catalog of a label from 1972 that somehow already knew what it was doing.

**Retro-futurism here means:** the future as imagined by people who had just landed on the moon and believed precision was a form of poetry. Not synthetic retro — not pastiche. The aesthetic logic of the Apollo program applied to the release of ambient music in 2026.

---

### The Graphic Atom — The Precision Circle

The circle is the label's primary graphic element. Not a glowing orb, not an atmospheric blob — a **geometric precision circle**: a perfect ring rendered as a measuring instrument. It references:

- **The sextant** — a navigational instrument for calculating position using celestial bodies
- **The telescope aperture** — the precise circular field of astronomical observation
- **The Bauhaus circle** — Kandinsky's geometric primary, elemental form
- **The lunar disc** — the moon seen not as mystery but as a measurable object: diameter 3,474 km, distance 384,400 km, orbital period 27.3 days

**Construction rules:**
- Outer ring: 1.5px stroke, accent color at 70% opacity
- Inner concentric ring (optional): 0.5px stroke, accent at 20% opacity — creates depth through geometry, not atmosphere
- Cardinal tick marks at 0°, 90°, 180°, 270° — instrument notation
- Phase body: the gibbous or crescent fill at max 10% opacity — present as data, not as drama
- No bloom. No glow. If light appears, it is because the form is precise, not because a filter was applied.

**Phase notation** remains: each release carries a phase code (TMR-001 / WXG). The circle in the header shows the current astronomical phase. This is the only dynamic element in the system.

---

## 2. Color System

### Core Palette

| Token | Hex | Role |
|---|---|---|
| `void` | `#0C0C0C` | Primary background — warm near-black, no blue |
| `surface` | `#141414` | Base surface |
| `surface-low` | `#1C1C1C` | Elevated section |
| `surface-container` | `#252525` | Card / component background |
| `moon-sand` | `#C4B98A` | Primary accent — warm cream/gold, the actual color of lunar regolith in NASA photography |
| `on-void` | `#E8E4D8` | Primary text — warm off-white |
| `on-dim` | `rgba(232,228,216,0.55)` | Secondary text |
| `on-faint` | `rgba(232,228,216,0.3)` | Tertiary — metadata, ghost text |
| `on-ghost` | `rgba(232,228,216,0.12)` | Borders, dividers |
| `accent-border` | `rgba(196,185,138,0.2)` | Standard border using accent hue |
| `accent-dim` | `rgba(196,185,138,0.08)` | Accent fill for hover/active states |

### Color Rules

**Moon Sand (`#C4B98A`)** is the single accent. It is warm — the inverse of the cold silver in most space aesthetics. This is a deliberate position: the moon's surface, photographed by Armstrong, is not silver. It is warm beige, almost golden. The accent is forensically correct.

**Background is warm black, not blue-black.** `#0C0C0C` has no hue. `#080B12` (the previous direction) read as indigo. The distinction matters: blue is atmospheric, nostalgic. Warm black is a void — neutral, absolute, functional.

**No atmospheric gradients as primary features.** A very subtle radial gradient in hero sections is permitted as atmosphere — max `rgba(196,185,138,0.025)` at center. If the gradient is visible as a design element, it is too strong.

**Depth through surface tiers, not shadows:**
```
#0C0C0C (void)
  └── #141414 (surface)
        └── #1C1C1C (section bg)
              └── #252525 (card/component)
```

### What Is Forbidden in Color

- Blue or purple tints in any background tone
- Cold accent colors — the accent is always warm
- Multiple accent colors
- Pure white (`#FFFFFF`) — `#E8E4D8` is the maximum brightness
- Warm gradients as decorative elements (they are permitted only as barely-perceptible atmosphere)

---

## 3. Typography

### Type Scale

| Role | Font | Weight | Size | Case |
|---|---|---|---|---|
| Hero / Display | Josefin Sans | 700 | clamp(4rem, 11vw, 8rem) | UPPERCASE |
| Headline | Josefin Sans | 600 | 1.5rem–2.5rem | UPPERCASE |
| Label / Navigation | Josefin Sans | 400 | 0.7rem–0.875rem | UPPERCASE |
| Body / Editorial | Josefin Sans | 300 | 0.95rem–1.05rem | Mixed |
| Data / Technical | Space Mono | 400 | 0.6rem–0.8rem | UPPERCASE |

### Font Sources

```
Josefin Sans: Google Fonts (free, open source)
— Weights used: 300 (Light), 400 (Regular), 600 (SemiBold), 700 (Bold)
— Explicitly geometric — Futura DNA, Bauhaus affiliation
— Thin strokes hold detail at display scale

Space Mono: Google Fonts
— Weight: 400 only
— Monospaced — technical data, catalog codes, phase notation, coordinates
— The aesthetic of mission documentation
```

### Typography Rules

**Josefin Sans is the label font** — all display, headline, navigation, and editorial text. It is a strictly geometric sans-serif, designed explicitly with Futura and the Bauhaus tradition as references. In uppercase at display scale, it reads like a mission header. In light weight at body scale, it has unexpected elegance.

**Space Mono is data** — every technical element: catalog numbers (TMR-001/FM), phase codes (WXG/74%), durations (38:14), timestamps, coordinates. The monospaced grid of Space Mono creates structural rhythm in dense data blocks.

**Uppercase is the default register for the label.** All navigation, all section labels, all UI elements: uppercase. The only exception is editorial body copy (release descriptions, label statement text) where mixed case is permitted. This distinction is functional: it separates the system voice from the content voice.

**Tracking discipline:**
- Hero display: `0.04em–0.08em` — Josefin Sans Bold is already tight at scale; do not over-track
- Navigation / uppercase labels: `0.18em–0.28em` — air creates legibility
- Space Mono: `0.08em` — slight tracking improves readability in uppercase mono

**Line-height:**
- Display: `0.9–1.0` — tight, column-like
- Body: `1.8–2.0` — generous, the space of slow reading

---

## 4. Language Convention

### Navigation and UI Labels

All uppercase, Josefin Sans, no underscores, spaces permitted:

```
THE MOON RECORDS
RELEASES
FIELD
ARCHIVE
CURRENT PHASE
SUBSCRIBE
```

### Technical / Data Labels

Space Mono, all uppercase, tight:

```
TMR-001 / WXG
PHASE  WXG / 74%
DURATION  44:22
SPECTRUM  DRONE / DEEP
EST. 2024
BEATLESS
```

### Section Number System (Bauhaus/Constructivist)

Sections are numbered in sequence, Space Mono, displayed as a structural element:

```
01 ─────────────────────────── CURRENT PHASE
02 ─────────────────────────── FIELD
03 ─────────────────────────── SUBSCRIBE
```

The number is not decorative. It is the label's acknowledgment that the page is a document — ordered, systematic, committed to structure.

### Copy Register

Precise. Declarative. No marketing language. No superlatives.

**Right:** "The Moon Records is a beatless label."
**Wrong:** "Immersive sonic journeys for discerning listeners."

---

## 5. Visual Effects System

### Film Grain (Retained, Minimal)

Very light grain overlay maintains the analog quality of mission photography:

```css
body::after {
  content: '';
  position: fixed; inset: 0;
  background-image: url("data:image/svg+xml, [noise SVG] ");
  opacity: 0.035;
  pointer-events: none;
  z-index: 9999;
  mix-blend-mode: overlay;
}
```

This is the only atmospheric effect. It is barely perceptible. Its purpose is to prevent the surface from reading as a clean digital render.

### Vignette (Retained, Restrained)

```css
.vignette {
  background: radial-gradient(ellipse at center, transparent 40%, rgba(12,12,12,0.5) 100%);
}
```

Intensity reduced from the previous version. The vignette is not the atmosphere — the geometry is.

### Horizontal Rules (Primary Structural Element)

Thin `1px` lines in `rgba(196,185,138,0.2)` define section boundaries. These are the visual equivalent of Bauhaus column rules — functional, not decorative:

```css
.rule { border-top: 1px solid rgba(196,185,138,0.2); }
```

Every major section transition uses a rule. Section headers sit at the rule. The grid is present.

### No Atmospheric Glow

The previous system used cold silver bloom on accent elements. This system does not. Accent elements are precise strokes and fills — they do not bleed into their surroundings. The precision circle may carry an extremely subtle SVG `feGaussianBlur` (stdDeviation ≤ 1.5) to smooth the anti-aliasing — not as atmosphere, but as rendering quality.

### Transitions

All transitions: `200ms–400ms ease-out`. Faster than the previous system (which used 600ms ease-in-out). Bauhaus is not soft. It does not hesitate.

---

## 6. Component Rules

### Border-Radius: Zero

No exceptions. Everything is a right angle.

### Buttons

**Primary:** `background: #C4B98A`, `color: #0C0C0C`, `border: none`. The filled shape. Bauhaus uses solid forms as primary communication. Hover: `background: #D4C99A` (lighter), `transform: none`. No glow.

**Ghost:** `background: transparent`, `border: 1px solid rgba(196,185,138,0.35)`, `color: #C4B98A`. Hover: `border-color: rgba(196,185,138,0.7)`.

No box-shadows on buttons. No bloom.

### Input Fields

`background: transparent`. `border-bottom: 1px solid rgba(196,185,138,0.3)` only. Focus: `border-color: #C4B98A`. No full border on focus.

### Release Cards

Cover art: `filter: grayscale(0.4) brightness(0.85)` at rest → `filter: none` on hover. The image reveals itself — not dramatically (no cross-fade from total grayscale), just a functional tonal correction.

No card border. Background surface tier shift distinguishes the card from the page.

### Section Numbers

Every section header carries a sequence number in Space Mono. The number appears as a structural element — above or to the left of the section title, separated by a horizontal rule fragment.

### Precision Circle Component

The circle SVG is a reusable component with a `phase` prop (0–1, illumination percentage). It renders:
1. Ghost circle (full outline, 10% opacity) — the whole moon, always visible
2. Outer ring (precise stroke, 70% opacity)
3. Phase body (very low opacity fill, appropriate arc for illumination)
4. Cardinal tick marks (4×, 50% opacity)
5. Optional: degree labels in Space Mono at 8px, 40% opacity

The component appears in the header (20px), on release cards (32px), in the subscribe section (24px), and as the hero graphic (360px+).

---

## 7. Layout Principles

**Grid first.** The 24px base unit governs all spacing. Every margin, padding, and gap derives from multiples of 24px.

**Asymmetric by design.** The hero composition: text left, circle right. Content does not center itself unless it is truly a solitary element (subscribe form, manifesto block).

**Horizontal rules as structure.** Where a section begins, there is a rule. Where a section header ends, there is a rule. The grid is legible.

**Section numbering creates document logic.** The page is a numbered document, not a scroll of impressions.

**Max-width: `56rem`** for content sections. Full-bleed for hero and full-width divider sections.

**Type size as hierarchy.** The scale jump between hero display (7rem+) and section title (1.5rem) is intentional and large. The contrast is the hierarchy.

---

## 8. What Is Forbidden

- Serif typefaces anywhere in the system
- Blue or cool-toned backgrounds
- Atmospheric gradient backgrounds (as primary features)
- Rounded corners on any element
- Glow / bloom effects (the previous "lunar silver bloom" is gone)
- Soft, slow transitions (anything over 400ms)
- Multiple accent colors
- Centered page layouts (hero and full-page moments excepted)
- Decorative illustration — only geometric, structural, and phase-coded graphics
- Pure white (`#FFFFFF`)
- Marketing copy register

---

## 9. Asset Checklist

Before any visual asset is released:

- [ ] Works in monochrome
- [ ] Could not be mistaken for another label
- [ ] Phase arc is geometrically precise — no freehand quality
- [ ] All text is Josefin Sans or Space Mono — no other typeface
- [ ] Uppercase in all system/UI elements
- [ ] No rounded corners
- [ ] No glow effects
- [ ] Background is warm black, not blue
- [ ] Accent is warm, not cold
- [ ] Grid unit is 24px — spacing is a multiple of 6px
- [ ] Section numbers are present where applicable
- [ ] Horizontal rules define section boundaries
- [ ] The design holds up at 100px wide
- [ ] The design holds up printed black and white
