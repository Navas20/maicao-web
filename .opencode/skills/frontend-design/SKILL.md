---
name: frontend-design
description: Use when writing HTML/CSS for web pages — layout, responsive design, CSS architecture, semantic HTML, forms, and performance patterns. Not for JavaScript logic, API design, or backend concerns.
---

# Frontend Design

## Core Principle

Write CSS that is **declarative, maintainable, and performant**. HTML must be **semantic and accessible** before any styling. Layout should work without JavaScript.

## CSS Reset

Always start with a minimal reset:

```css
*, *::before, *::after {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
img { max-width: 100%; display: block; }
input, button, textarea, select { font: inherit; }
```

## CSS Custom Properties

Define a design token system before writing component styles:

```css
:root {
  /* Colors */
  --color-primary: #C62828;
  --color-primary-hover: #8E1B1B;
  --color-surface: #ffffff;
  --color-bg: #fafafa;
  --color-text: #212121;
  --color-text-secondary: #666666;
  --color-border: #e0e0e0;

  /* Spacing */
  --space-xs: 4px; --space-sm: 8px;
  --space-md: 16px; --space-lg: 24px;
  --space-xl: 32px; --space-2xl: 48px; --space-3xl: 64px;

  /* Typography */
  --font-body: 'Inter', system-ui, sans-serif;
  --font-heading: 'Playfair Display', Georgia, serif;
  --text-sm: 0.875rem;
  --text-base: 1rem;
  --text-lg: 1.25rem;
  --text-xl: 1.5rem;

  /* Layout */
  --max-width: 1280px;
  --header-height: 72px;

  /* Other */
  --radius: 12px;
  --shadow: 0 4px 24px rgba(0,0,0,.08);
}
```

## Layout Patterns

### Fixed Header
```css
header {
  position: fixed; top: 0; left: 0; right: 0;
  height: var(--header-height);
  z-index: 1000;
  background: rgba(255,255,255,.95);
  backdrop-filter: blur(12px);
}
/* Offset for anchor links */
html { scroll-padding-top: var(--header-height); }
```

### Responsive Grid
Use `auto-fit` + `minmax` to avoid media queries:

```css
.grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(min(100%, 280px), 1fr));
  gap: var(--space-lg);
}
```

### Section Container
```css
.section-inner {
  max-width: var(--max-width);
  margin-inline: auto;
  padding-inline: var(--space-lg);
}
```

## Semantic HTML

| Element | Use for | Not for |
|---|---|---|
| `<header>` | Site/banner header, article intro | Just a div with a class |
| `<nav>` | Primary navigation links | Non-navigation lists |
| `<main>` | Primary page content | Sidebars, ads |
| `<section>` | Thematic content grouping | Styling containers |
| `<article>` | Self-contained content | Generic cards |
| `<aside>` | Complementary content | Sidebars with primary content |
| `<footer>` | Footer for page or section | Bottom bars |

### Anchor Links with Fixed Header
```css
html { scroll-padding-top: 80px; }
```

## Responsive Design

### Content-Based Breakpoints
```css
/* Base: mobile */
.grid { grid-template-columns: 1fr; }

/* Content widens → columns increase */
@media (min-width: 640px) {
  .grid { grid-template-columns: repeat(2, 1fr); }
}
@media (min-width: 960px) {
  .grid { grid-template-columns: repeat(3, 1fr); }
}
```

### Recommended Breakpoints
| Name | Width | Changes |
|---|---|---|
| Mobile | < 640px | Single column, stacked |
| Tablet | 640-959px | 2 columns, visible nav |
| Desktop | 960-1279px | 3 columns, full layout |
| Wide | ≥ 1280px | Max-width container |

### Mobile Non-Negotiables
- Tap targets ≥ 44×44px
- Font size ≥ 16px (prevents iOS zoom)
- No horizontal scroll
- Full-width buttons on forms

## Forms

```html
<div class="field">
  <label for="name">Nombre completo</label>
  <input type="text" id="name" name="name"
         placeholder="Ej: Juan Pérez" required
         aria-describedby="name-error">
  <span id="name-error" class="error-msg" role="alert"></span>
</div>
```

```css
.field {
  display: flex; flex-direction: column;
  gap: var(--space-xs);
}
.field label { font-weight: 600; font-size: 0.875rem; }
.field input, .field textarea, .field select {
  padding: 0.75rem 1rem;
  border: 1px solid var(--color-border);
  border-radius: var(--radius-sm, 8px);
  font-size: 1rem; /* prevent iOS zoom */
  transition: border-color 200ms, box-shadow 200ms;
}
.field input:focus-visible {
  outline: none;
  border-color: var(--color-accent);
  box-shadow: 0 0 0 3px rgba(255,179,0,.2);
}
.field .error-msg { color: #EF5350; font-size: 0.8125rem; display: none; }
.field .error-msg.visible { display: block; }
.field input.error { border-color: #EF5350; }
```

## Buttons

```css
.btn {
  display: inline-flex; align-items: center; gap: 8px;
  padding: 0.75rem 1.5rem;
  border-radius: 8px;
  font-weight: 600; font-size: 0.9375rem;
  border: none; cursor: pointer;
  text-decoration: none;
  transition: transform 200ms, box-shadow 200ms, background 200ms;
}
.btn:hover { transform: translateY(-2px); }
.btn:active { transform: translateY(0); }
.btn:focus-visible {
  outline: 2px solid var(--color-accent);
  outline-offset: 3px;
}
.btn:disabled {
  opacity: .5; cursor: not-allowed;
  transform: none;
}
```

## CSS Performance

| Practice | Why |
|---|---|
| Animate only `transform`, `opacity` | GPU composited, no layout |
| Avoid `@import` in CSS | Blocks rendering |
| Use `content-visibility: auto` | Skips rendering below fold |
| Use `will-change` sparingly | Only on continuously animating elements |
| Keep specificity low | Prevents overrides, smaller CSS |
| Use logical properties | `margin-inline`, `padding-block` |

## Font Loading

```html
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;600;700&display=swap" rel="stylesheet">
```

```css
body { font-family: 'Inter', system-ui, sans-serif; }
```

Use `font-display: swap` (default with `display=swap`) to show fallback font immediately.

## Accessibility in CSS

```css
/* Skip link */
.skip-link {
  position: absolute; top: -100%;
  left: 0; padding: 8px 16px;
  background: #fff; z-index: 9999;
}
.skip-link:focus { top: 0; }

/* Focus styles */
:focus-visible {
  outline: 2px solid var(--color-accent);
  outline-offset: 2px;
}
/* Never do: */
/* :focus { outline: none; } (without replacement) */

/* Respect user preferences */
@media (prefers-reduced-motion: reduce) {
  *, *::before, *::after {
    animation-duration: 0.01ms !important;
    transition-duration: 0.01ms !important;
  }
}
```

## Common Mistakes

| Mistake | Fix |
|---|---|
| `box-sizing` not set globally | Add `*, *::before, *::after { box-sizing: border-box; }` |
| Anchors hidden under fixed header | Add `html { scroll-padding-top: var(--header-height); }` |
| `outline: none` without replacement | Use `:focus-visible` with explicit styles |
| `transition: all` | Be explicit about properties |
| Placeholder as label | Use visible `<label>` |
| No `max-width` on containers | Use `max-width: 1280px; margin-inline: auto;` |
| Images without `max-width: 100%` | Overflow on mobile |
| Desktop-first media queries | Write mobile-first (`min-width`) |
| Hardcoded pixel values | Use CSS custom properties |
| No focus indicators | Always add `:focus-visible` styles |
