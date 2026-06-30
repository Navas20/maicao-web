---
name: impeccable-design-taste
description: Use when polishing a UI to move from "good enough" to "feels professional" — refinement of spacing, color harmony, typography, shadows, micro-copy, and visual consistency. Not for structural layout or accessibility requirements (covered by ui-ux-pro and frontend-design skills).
---

# Impeccable Design & Taste

## Core Principle

Great design is invisible. The user shouldn't notice any single element — they should feel the page is trustworthy, clear, and intentional. "Taste" is knowing what to remove.

## What Separates Pro From Amateur

| Amateur | Professional |
|---|---|
| Pure black `#000` | Off-black `#1a1a1a` or `#212121` |
| Pure white `#fff` everywhere | Off-white `#fafafa` or `#f8f8f8` for backgrounds |
| Shadows with `0 0` offset | Shadows with vertical offset + blur |
| Random border-radius | Consistent border-radius scale |
| Multiple competing colors | Controlled palette with 60-30-10 ratio |
| Simultaneous animations | Staggered, choreographed motion |
| Text touching edges | Generous padding, breathing room |
| Icon sets mixed (filled + outlined) | Icon family consistency |
| Hard pixel values | Proportional spacing scale |

## Color Harmony

### The 60-30-10 Ratio
- **60%** neutral background (whites, grays)
- **30%** primary brand color (headers, surfaces)
- **10%** accent (CTAs, highlights, links)

### Color Temperature Balance
Warm palettes feel energetic but tire the eye. Cool palettes feel calm but can feel cold. Mix them:

```
Warm dominant page → add cool accent in shadows/secondary elements
Cool dominant page → add warm accent in CTAs/interactive elements
```

### Never Use Pure Black
Pure black `#000` doesn't exist in nature. Use:
| Use case | Replacement |
|---|---|
| Text | `#1a1a1a` or `#212121` |
| Dark footer | `#0d0d1a` or `#1a1a2e` |
| Dark button | `#2d2d3a` not `#000` |
| Light shadow | `rgba(0,0,0,.08)` not `#000` |
| Heavy shadow | `rgba(0,0,0,.15)` not `#000` |

### Shadow Realism
```css
/* ❌ Amateur: floaty, no depth cue */
box-shadow: 0 0 20px rgba(0,0,0,0.15);

/* ✅ Professional: light source from above */
box-shadow: 0 2px 8px rgba(0,0,0,.06);   /* subtle */
box-shadow: 0 4px 24px rgba(0,0,0,.08);  /* medium */
box-shadow: 0 8px 40px rgba(0,0,0,.12);  /* elevated */
```

The vertical offset simulates a light source from above (standard UI convention).

## Typography Taste

### Pairing
Pair a serif display font with a sans-serif body font:
- **Display + Body:** Playfair Display + Inter
- **Display + Body:** Georgia + system-ui
- **Display + Body:** Merriweather + Open Sans

### Size Rhythm
Use modular scale. Common ratios:

| Ratio | Use case |
|---|---|
| 1.250 (major second) | Dense information pages |
| 1.333 (perfect fourth) | Balanced content pages |
| 1.500 (perfect fifth) | Editorial / storytelling |

```css
--text-sm: 0.875rem;    /* 14px */
--text-base: 1rem;      /* 16px */
--text-lg: 1.25rem;     /* 20px */
--text-xl: 1.5rem;      /* 24px */
--text-2xl: 2rem;       /* 32px */
--text-3xl: 2.5rem;     /* 40px */
--text-4xl: 3rem;       /* 48px */
```

### Leading (Line Height)
| Content type | Line height |
|---|---|
| Body text | 1.6 - 1.8 |
| Headings | 1.05 - 1.2 |
| Small text (< 14px) | 1.5 |
| Lists | 1.6 |

## Spacing & Breathing Room

### The 8px Grid
All spacing should be multiples of 8px (or 4px for micro spacing):

```css
--space-2xs: 2px;
--space-xs: 4px;
--space-sm: 8px;
--space-md: 16px;
--space-lg: 24px;
--space-xl: 32px;
--space-2xl: 48px;
--space-3xl: 64px;
--space-4xl: 80px;
```

### Section Spacing
- Desktop: 80-120px vertical padding
- Mobile: 48-64px vertical padding
- Card padding: 20-32px (never less than 16px)

### The "Touching Text" Test
If any text is closer to an edge/container than to the next text element, increase padding.

## Visual Rhythm

### Alternating Backgrounds
Sections should alternate background colors to give visual breathing:

```
Section 1: #fff (white)
Section 2: #f8f8f8 (light gray)
Section 3: #fff (white)
Section 4: #f0f0f0 (slightly darker)
```

This creates natural visual breaks without needing visible dividers.

### Content Density
| Density | Characters per line | Use for |
|---|---|---|
| Relaxed | 60-65ch | Reading-heavy (articles) |
| Moderate | 65-75ch | Mixed (landing pages) |
| Dense | 75-85ch | Data-heavy (dashboards) |

## Border Radius Scale

```css
--radius-sm: 6px;    /* buttons, inputs */
--radius-md: 10px;   /* cards, containers */
--radius-lg: 16px;   /* modals, large sections */
--radius-xl: 24px;   /* hero cards, special elements */
```

Use max 3 radius sizes across a page. Consistency creates polish.

## Icon Taste

| Rule | Why |
|---|---|
| Pick one icon family (Font Awesome, Lucide, Heroicons) | Mixed families look disjointed |
| Use same weight throughout (regular, solid, or outline) | Mixed weights look sloppy |
| Consistent size within sections | Uneven sizes look unplanned |
| Align icons to text baseline | Optical alignment matters |

## The "Why" Test

Before adding any visual element, ask:
1. Does this help the user understand something?
2. Does this guide their attention?
3. Does this make the page feel more trustworthy?

If "no" to all three, remove it.

## Polish Checklist

- [ ] No pure `#000` or pure `#fff` (use off-black, off-white)
- [ ] Shadows have vertical offset, not `0 0`
- [ ] Border-radius consistent (max 3 values)
- [ ] Spacing follows 4px/8px grid
- [ ] At least 3 alternating section backgrounds
- [ ] Text never touches edges
- [ ] Icons same weight and family throughout
- [ ] Line height ≥ 1.6 for body text
- [ ] Card padding ≥ 16px
- [ ] Max line length ≤ 75ch
- [ ] Font pairing: display + body, distinct
- [ ] No competing gradients per section

## Common Taste Mistakes

| Mistake | Why it's bad | Fix |
|---|---|---|
| `#000` footer | Looks heavy, cheap | Use `#0d0d1a` or `#1a1a2e` |
| `box-shadow: 0 0` | Floaty, unrealistic | Add vertical offset |
| Every card different gradient | Visual noise | Unify icon containers |
| Mixed icon styles | Sloppy, unplanned | Stick to one set/weight |
| Text touching card edge | Claustrophobic | Add `padding: 24px` minimum |
| Uneven spacing | Feels random | Use spacing scale |
| Too many font sizes | No hierarchy | Use 4-5 sizes max |
| Same background all sections | Blends together | Alternate every section |
