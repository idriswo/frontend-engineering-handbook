# Performance

## Purpose

This chapter defines how we keep the app fast: code splitting, lazy loading, memoization, render optimization, and measuring before optimizing.

## Why It Matters

Performance is a feature users feel on every interaction. Slow bundles delay first paint; unnecessary re-renders cause jank; unoptimized lists freeze the UI. But premature optimization wastes effort and complicates code. The discipline is: measure first, optimize the real bottleneck, and keep the fast path simple.

## Core Concepts

**Code splitting.** Split routes and heavy components with `React.lazy` + `Suspense`.

**Memoization.** `React.memo`, `useMemo`, `useCallback` prevent needless recomputation/re-render — when justified by measurement.

**Referential stability.** Stable props (functions, objects) prevent memoized children from re-rendering.

**Measure first.** Use React DevTools Profiler and Lighthouse before optimizing.

## Rules

1. Lazy-load routes and heavy components; wrap in `<Suspense>`.
2. Do not memoize by default — profile, then memoize proven hotspots.
3. Keep referential stability for props passed to memoized children.
4. Virtualize large lists (hundreds+ of rows).
5. Avoid derived state and redundant effects that cause extra renders.
6. Measure with the Profiler/Lighthouse before and after optimizing.

## Best Practices

- Split vendor and route bundles; analyze bundle size.
- Debounce/throttle expensive event handlers (search, resize).
- Prefer derived values over synced state (fewer renders).

## Bad Examples

```tsx
// ❌ Memoizing everything blindly; new function prop breaks memo anyway.
const List = React.memo(function List({ items, onSelect }: Props) { /* ... */ });

<List items={items} onSelect={() => select(id)} /> // new fn each render → memo useless
```

## Good Examples

```tsx
// ✅ Lazy route + stable callback so memoized child actually skips re-render.
const Reports = lazy(() => import("@/features/reports"));

function Parent({ items }: { items: Item[] }) {
  const handleSelect = useCallback((id: string) => select(id), []);
  return (
    <Suspense fallback={<Skeleton />}>
      <MemoList items={items} onSelect={handleSelect} />
    </Suspense>
  );
}
```

**Real World Example:** `Reports` loads only when its route is visited, shrinking the initial bundle.

**Production Example:** A stable `handleSelect` lets `MemoList` skip re-rendering when unrelated parent state changes.

**Explanation:** Optimization works only when memo + referential stability are used together, on a measured hotspot.

## Common Mistakes

- Memoizing everything without measuring.
- Passing inline functions/objects to memoized children (defeats memo).
- Rendering huge lists without virtualization.
- Optimizing before profiling.

## AI Instructions

- Lazy-load routes/heavy components with Suspense.
- Only add memoization to measured hotspots, with stable props.
- Virtualize large lists.
- Prefer derived values over synced state.

## Checklist

- [ ] Are routes/heavy components code-split?
- [ ] Is memoization applied to measured hotspots (with stable props)?
- [ ] Are large lists virtualized?
- [ ] Was performance measured before/after?

## Summary

Performance is measured, not guessed. We code-split routes, memoize only proven hotspots with stable props, virtualize large lists, and minimize needless renders — keeping the app fast without premature complexity.

## 🔗 Related Chapters

Read next:

- 09-react.md
- 12-react-router.md
- 25-error-handling.md
