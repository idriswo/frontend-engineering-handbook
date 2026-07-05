# Error Handling

## Purpose

This chapter defines how we handle errors and async states: error boundaries, global error handling, toasts, and the mandatory loading/error/empty/success states for every data-driven view.

## Why It Matters

Unhandled errors crash the UI, confuse users, and hide failures from developers. Missing loading/empty states make the app feel broken on slow networks or with no data. Comprehensive error handling is what separates a demo from a production application — it is non-negotiable here.

## Core Concepts

**Four states.** Every async view handles **loading**, **error**, **empty**, and **success**.

**Error boundaries.** Catch render-time errors in a subtree and show a fallback instead of a white screen.

**Global handler.** Central place to log errors and surface user-friendly messages.

**Toasts.** Non-blocking feedback for recoverable errors and successes.

**Skeletons/empty states.** Communicate loading and no-data conditions clearly.

## Rules

1. Every data-driven view handles loading, error, empty, and success states.
2. Wrap route subtrees in an error boundary with a friendly fallback.
3. Normalize errors (chapter 16) so the UI shows consistent messages.
4. Use toasts for recoverable feedback; boundaries for render crashes.
5. Never swallow errors silently — log them and inform the user.
6. Show skeletons for loading and explicit empty states for no data.

## Best Practices

- Provide retry actions in error states.
- Log errors to a monitoring service in the global handler.
- Keep error messages actionable and non-technical for users.

## Bad Examples

```tsx
// ❌ No states, silent catch, crash on empty/error.
function List() {
  const { data } = useGetItemsQuery();
  return <ul>{data.map((i) => <li key={i.id}>{i.name}</li>)}</ul>;
}
```

## Good Examples

```tsx
// features/shared/ErrorBoundary.tsx
import { Component, type ReactNode } from "react";

export class ErrorBoundary extends Component<
  { children: ReactNode; fallback: ReactNode },
  { hasError: boolean }
> {
  state = { hasError: false };
  static getDerivedStateFromError() {
    return { hasError: true };
  }
  componentDidCatch(error: unknown) {
    logError(error);
  }
  render() {
    return this.state.hasError ? this.props.fallback : this.props.children;
  }
}
```

```tsx
// ✅ All four states handled.
function ItemList() {
  const { data: items, isLoading, isError, refetch } = useGetItemsQuery();

  if (isLoading) return <ListSkeleton />;
  if (isError) return <ErrorState onRetry={refetch} message="Couldn't load items." />;
  if (!items?.length) return <EmptyState message="No items yet." />;

  return (
    <ul>
      {items.map((item) => (
        <li key={item.id}>{item.name}</li>
      ))}
    </ul>
  );
}
```

**Real World Example:** The error boundary (a rare justified class component) keeps a widget crash from blanking the whole page.

**Production Example:** On a flaky connection users see a skeleton, then a retryable error — never a blank screen or a crash.

**Explanation:** Four-state handling plus boundaries make failures graceful and observable.

## Common Mistakes

- Rendering data without loading/error/empty guards.
- Swallowing errors in empty `catch` blocks.
- Technical error messages shown to users.
- No retry path from error states.

## AI Instructions

- Handle loading, error, empty, and success in every async view.
- Wrap risky subtrees in an error boundary with a fallback.
- Normalize and log errors; show friendly, retryable messages.
- Never leave a silent `catch`.

## Checklist

- [ ] Are all four async states handled?
- [ ] Is the subtree wrapped in an error boundary?
- [ ] Are errors logged and normalized?
- [ ] Are messages user-friendly with a retry path?

## Summary

Robust error handling means four states everywhere (loading/error/empty/success), error boundaries for render crashes, a global handler for logging, and toasts for feedback. Errors are never swallowed; users always see a clear, recoverable state.

## 🔗 Related Chapters

Read next:

- 14-rtk-query.md
- 24-performance.md
- 26-testing.md
