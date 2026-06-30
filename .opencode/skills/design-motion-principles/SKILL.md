---
name: design-motion-principles
description: Use when animating UI elements — hover states, entry/exit transitions, loading states, scroll animations, or microinteractions. Covers easing, duration, stagger, performance, accessibility, and choreography. Not for video editing, 3D animation, or game development.
---

# Design Motion Principles

## Core Principle

Motion in UI must be **functional**, not decorative. Every animation answers: *does this help the user understand what happened, what will happen, or where to look?*

## The 5 UI Motion Goals

| Goal | Example | Why |
|---|---|---|
| **Orient** | List → Detail transition | User knows where they are |
| **Direct attention** | Shimmer on loading card | User knows content is coming |
| **Show causality** | Button expands to panel | User knows what caused it |
| **Provide feedback** | Hover lift | Element is interactive |
| **Express brand** | Logo animation | Personality without distraction |

## Easing

Never use `linear` for UI motion. It looks robotic and feels unnatural.

| Easing | Timing function | Use for |
|---|---|---|
| **ease-out** | `cubic-bezier(0, 0, 0.2, 1)` | Entries: elements appearing |
| **ease-in** | `cubic-bezier(0.4, 0, 1, 1)` | Exits: elements leaving (rare) |
| **ease-in-out** | `cubic-bezier(0.4, 0, 0.2, 1)` | Emphasis: pulsing, attention |
| **Material standard** | `cubic-bezier(0.4, 0, 0.2, 1)` | Most UI transitions |
| **Material decelerate** | `cubic-bezier(0, 0, 0.2, 1)` | Elements entering screen |
| **Material accelerate** | `cubic-bezier(0.4, 0, 1, 1)` | Elements leaving screen |
| **Snap** | `cubic-bezier(0.34, 1.56, 0.64, 1)` | Fun microinteractions (bounce) |

```css
:root {
  --ease-out: cubic-bezier(0, 0, 0.2, 1);
  --ease-in: cubic-bezier(0.4, 0, 1, 1);
  --ease-in-out: cubic-bezier(0.4, 0, 0.2, 1);
  --ease-snap: cubic-bezier(0.34, 1.56, 0.64, 1);
}
```

## Duration

| Interaction | Duration | Easing |
|---|---|---|
| Hover (micro) | 150-200ms | ease-out |
| Focus ring | 150-200ms | ease-out |
| Button press | 100-150ms | ease-out |
| Entry animation | 300-500ms | ease-out |
| Exit animation | 200-300ms | ease-in |
| Page transition | 300-400ms | ease-in-out |
| Modal open | 200-300ms | ease-out |
| Notification | 250-400ms | ease-out |
| Loading shimmer | 1000-1500ms loop | linear |
| Progress bar | 300-500ms | ease-out |

### Duration Rule
- **Under 100ms:** User may miss it
- **100-300ms:** Feels instant, responsive
- **300-500ms:** Notices but doesn't wait
- **500-1000ms:** Feels slow, needs purpose
- **Over 1000ms:** Only for loading/emphasis

## Stagger & Sequencing

Elements entering a group should not appear simultaneously. Stagger creates a visual wave:

```css
.card:nth-child(1) { animation-delay: 0ms; }
.card:nth-child(2) { animation-delay: 80ms; }
.card:nth-child(3) { animation-delay: 160ms; }
```

| Grid size | Stagger per item |
|---|---|
| 2-3 items | 80-100ms |
| 4-6 items | 60-80ms |
| 7+ items | 40-60ms |

Total animation time should not exceed 800ms even for large groups.

## FLIP Technique

For layout animations (elements moving between positions), use FLIP:

```js
// First, Last, Invert, Play
function flip(el, callback) {
  const first = el.getBoundingClientRect();
  callback();
  const last = el.getBoundingClientRect();
  const dx = first.left - last.left;
  const dy = first.top - last.top;
  const dw = first.width / last.width;
  const dh = first.height / last.height;
  el.animate([
    { transform: `translate(${dx}px, ${dy}px) scale(${dw}, ${dh})` },
    { transform: 'translate(0, 0) scale(1, 1)' }
  ], { duration: 300, easing: 'ease-out' });
}
```

## Performance

Always animate **transform** and **opacity** only. Never animate `width`, `height`, `top`, `left`, `margin`, or `padding` — they trigger layout recalculations.

```css
/* ❌ Bad — triggers layout + paint */
.card { transition: left 300ms, width 300ms; }

/* ✅ Good — GPU composited */
.card { transition: transform 300ms, opacity 300ms; }
```

### will-change
Use sparingly, only on elements that animate continuously:

```css
.card { will-change: transform, opacity; }
```

Never use on more than 10-15 elements simultaneously.

## Accessibility: prefers-reduced-motion

Always respect user's motion preferences:

```css
@media (prefers-reduced-motion: reduce) {
  *, *::before, *::after {
    animation-duration: 0.01ms !important;
    animation-iteration-count: 1 !important;
    transition-duration: 0.01ms !important;
  }
  html { scroll-behavior: auto; }
}
```

For critical transitions (tooltips, modals), use fade-only fallback:

```css
@media (prefers-reduced-motion: reduce) {
  .card {
    animation: fade-in 150ms ease-out;
  }
}
```

## Entry vs Exit Animations

| | Entry | Exit |
|---|---|---|
| Duration | 300-500ms | 150-250ms |
| Easing | ease-out | ease-in |
| Translate | From 20-40px | To -10-(-20)px |
| Opacity | 0 → 1 | 1 → 0 |
| Scale | 0.95 → 1 | 1 → 0.97 |

Exits should be faster than entries. User needs to see what appeared; they already know what's leaving.

## Skeleton Screens

Show structure before content. Animate with shimmer:

```css
.skeleton {
  background: linear-gradient(
    90deg,
    #e0e0e0 25%,
    #f5f5f5 50%,
    #e0e0e0 75%
  );
  background-size: 200% 100%;
  animation: shimmer 1.5s ease-in-out infinite;
  border-radius: 8px;
}

@keyframes shimmer {
  0% { background-position: 200% 0; }
  100% { background-position: -200% 0; }
}
```

Transition from skeleton to content: 200ms crossfade. Never snap-replace.

## Transition Checklist

Before shipping an animation:

- [ ] Uses `transform`/`opacity` only (not layout properties)
- [ ] Duration appropriate for interaction type
- [ ] Easing is not `linear`
- [ ] `prefers-reduced-motion` handled
- [ ] Entry animations delayed (staggered) in groups
- [ ] Exit animations faster than entry
- [ ] No animation on elements that appear within 300ms of page load
- [ ] Loading states use skeleton, not spinner
- [ ] Hover states use 150-200ms, not instant
- [ ] Focus ring transitions are smooth

## Common Mistakes

| Mistake | Fix |
|---|---|
| Linear easing everywhere | Use ease-out for entries, ease-in for exits |
| Animating `width`/`height` | Use `transform: scale()` |
| All items animate simultaneously | Add stagger delays |
| No reduced-motion support | Add `@media (prefers-reduced-motion: reduce)` |
| Loading spinner for content | Use skeleton + shimmer |
| 600ms hover transition | Use 150-200ms for hover |
| `transition: all 300ms` | Be explicit: `transition: transform 300ms, opacity 300ms` |
| Exit transitions omitted | Elements leaving need animation too |
