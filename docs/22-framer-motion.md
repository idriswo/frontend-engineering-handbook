# Framer Motion

## Purpose

This chapter defines how we add animation with **Framer Motion**: entrance/exit transitions, layout animations, and gestures — while respecting performance and accessibility.

## Why It Matters

Motion communicates state changes, guides attention, and makes an interface feel responsive. Done well it improves UX; done carelessly it harms performance, causes layout jank, and hurts users with motion sensitivity. This chapter keeps animation purposeful, performant, and accessible.

## Core Concepts

**Declarative motion.** Animate via `motion.*` components with `initial`/`animate`/`exit` props.

**`AnimatePresence`.** Enables exit animations for unmounting elements.

**Performant properties.** Animate `transform` and `opacity` (GPU-friendly), avoid animating layout-triggering properties.

**Reduced motion.** Respect `prefers-reduced-motion`.

## Rules

1. Animate `transform`/`opacity` for performance; avoid animating `width`/`height`/`top`/`left`.
2. Use `AnimatePresence` for exit animations of conditionally rendered elements.
3. Respect `prefers-reduced-motion`; provide non-animated fallbacks.
4. Keep animations short and purposeful (typically 150–300ms).
5. Do not animate large lists without virtualization considerations.

## Best Practices

- Extract reusable variants/transitions into shared constants.
- Use `layout` animations sparingly and test for jank.
- Pair motion with meaning (feedback, continuity), not decoration.

## Bad Examples

```tsx
// ❌ Animating layout props, no reduced-motion support, no exit handling.
<motion.div animate={{ width: open ? 400 : 0, height: open ? 300 : 0 }} />
```

## Good Examples

```tsx
import { motion, AnimatePresence, useReducedMotion } from "framer-motion";

export function Toast({ open, message }: { open: boolean; message: string }) {
  const reduce = useReducedMotion();

  return (
    <AnimatePresence>
      {open && (
        <motion.div
          role="status"
          initial={reduce ? false : { opacity: 0, y: 12 }}
          animate={{ opacity: 1, y: 0 }}
          exit={reduce ? { opacity: 0 } : { opacity: 0, y: 12 }}
          transition={{ duration: 0.2 }}
        >
          {message}
        </motion.div>
      )}
    </AnimatePresence>
  );
}
```

**Real World Example:** The toast slides and fades using GPU-friendly `opacity`/`y`, staying smooth at 60fps.

**Production Example:** Users with reduced-motion preferences get a simple fade, honoring accessibility settings.

**Explanation:** Animating transform/opacity plus reduced-motion support gives smooth, inclusive motion.

## Common Mistakes

- Animating layout-triggering properties (jank).
- Forgetting `AnimatePresence` for exit animations.
- Ignoring `prefers-reduced-motion`.
- Overusing animation as decoration.

## AI Instructions

- Animate `transform`/`opacity`, not layout properties.
- Use `AnimatePresence` for exit transitions.
- Respect `prefers-reduced-motion` with fallbacks.
- Keep animations short and purposeful.

## Checklist

- [ ] Are only performant properties animated?
- [ ] Is `AnimatePresence` used for exits?
- [ ] Is reduced motion respected?
- [ ] Is the animation purposeful and short?

## Summary

Framer Motion adds purposeful, performant, accessible animation. We animate transform/opacity, handle exits with `AnimatePresence`, respect reduced-motion preferences, and keep motion meaningful rather than decorative.
