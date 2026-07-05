# ✅ Performance Checklist

Reference: [24-performance.md](../docs/24-performance.md).

## Rendering

- [ ] No unnecessary re-renders (verified with React DevTools Profiler)
- [ ] Expensive computations wrapped in `useMemo` (only where profiling justifies it)
- [ ] Stable callbacks via `useCallback` where passed to memoized children
- [ ] Lists use stable unique keys (never index on mutable lists)
- [ ] Large lists virtualized (e.g. `react-window`) when needed
- [ ] State kept as local as possible to limit render scope

## Loading & Bundles

- [ ] Route-level code splitting with `React.lazy` + `Suspense`
- [ ] Heavy dependencies lazy-loaded or replaced with lighter alternatives
- [ ] Images optimized (correct format, sizes, `loading="lazy"`)
- [ ] Bundle analyzed; no accidental large imports (e.g. whole lodash)
- [ ] Tree-shaking works (named imports, ESM)

## Data

- [ ] Server data cached via RTK Query — no redundant refetches
- [ ] Requests debounced/throttled where appropriate (search, resize, scroll)
- [ ] Pagination or infinite scroll for large datasets

## Metrics

- [ ] Core Web Vitals in the green (LCP, CLS, INP)
- [ ] Lighthouse performance score acceptable for the app's target

---

**Rule of thumb:** measure first, optimize the real hotspot — never guess.
