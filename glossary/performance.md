# Performance Glossary

Performance terms and metrics. See [24-performance.md](../docs/24-performance.md) and [checklists/performance.md](../checklists/performance.md).

---

## Re-render
React re-running a component to produce new UI after state/props change. Excessive re-renders are a common perf issue.

## Memoization
Caching a computed result to skip recomputation: `useMemo` (values), `useCallback` (functions), `React.memo` (components).

## Code Splitting
Breaking the bundle into chunks loaded on demand, typically per route via `React.lazy`.

## Lazy Loading
Deferring loading of code, images, or data until they're actually needed.

## Tree Shaking
Build-time elimination of unused (dead) code, enabled by ES modules and named imports.

## Bundle Size
The total JavaScript shipped to the browser. Smaller bundles = faster load.

## Virtualization
Rendering only the visible portion of a long list (windowing) to keep the DOM small.

## Debounce / Throttle
Rate-limiting techniques for frequent events. Debounce waits for a pause; throttle caps frequency.

## Core Web Vitals
Google's UX metrics:
- **LCP** (Largest Contentful Paint) — loading
- **CLS** (Cumulative Layout Shift) — visual stability
- **INP** (Interaction to Next Paint) — responsiveness

## Critical Rendering Path
The sequence of steps the browser takes to convert HTML/CSS/JS into painted pixels.

## Caching
Reusing previously fetched data. RTK Query caches server responses and invalidates them via tags.

## Reflow / Repaint
Reflow recalculates layout (expensive); repaint redraws pixels. Minimize layout thrashing.
