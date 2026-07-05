# React

## Purpose

This chapter defines how we write **React 19** components: functional components, hooks, composition, and the modern React model. It is the core of the frontend stack.

## Why It Matters

React is the foundation every feature is built on. Consistent React patterns make components predictable, performant, and reusable. Misused hooks, uncontrolled re-renders, and tangled state are the most common sources of frontend bugs — this chapter prevents them.

## Core Concepts

**Functional components only.** No class components. State and lifecycle come from hooks.

**Hooks rules.** Call hooks at the top level, never conditionally; custom hooks encapsulate reusable logic.

**Composition over inheritance.** Build UIs by composing small components and passing `children`.

**Derived state, not duplicated state.** Compute from props/state during render instead of syncing with effects.

**Effects are for synchronization with external systems**, not for deriving values.

## Rules

1. Use functional components with typed props.
2. Never use class components.
3. Follow the Rules of Hooks; no conditional hook calls.
4. Extract reusable stateful logic into custom hooks.
5. Prefer derived values over `useState` + `useEffect` syncing.
6. Every list item needs a stable, unique `key` (never the index when the list mutates).
7. Keep components small and single-purpose.

## Best Practices

- Lift state only as high as needed; keep it local otherwise.
- Split presentational and container concerns (see SOLID).
- Memoize expensive computations with `useMemo`, stable callbacks with `useCallback` — only when profiling justifies it.
- Prefer controlled components for forms (with React Hook Form).

## Bad Examples

```tsx
// ❌ Class component, index keys, effect-derived state.
class List extends React.Component {
  render() {
    return this.props.items.map((item, i) => <li key={i}>{item.name}</li>);
  }
}
```

## Good Examples

```tsx
// ✅ Functional, typed, stable keys, derived value.
interface Item {
  id: string;
  name: string;
  price: number;
}

function ItemList({ items }: { items: Item[] }) {
  const total = items.reduce((sum, item) => sum + item.price, 0);

  return (
    <div>
      <ul>
        {items.map((item) => (
          <li key={item.id}>{item.name}</li>
        ))}
      </ul>
      <p>Total: {total}</p>
    </div>
  );
}
```

**Real World Example:** `total` is derived during render — no effect, no stale state, no extra re-render.

**Production Example:** Stable `item.id` keys keep the list correct and performant when items are reordered or removed.

**Explanation:** Functional components + derived state + stable keys eliminate the classic re-render and stale-state bugs.

## Common Mistakes

- Using `useEffect` to copy props into state.
- Index keys on mutable lists.
- Over-memoizing before measuring.
- Giant components mixing many responsibilities.

## AI Instructions

- Generate only functional, typed components.
- Follow the Rules of Hooks strictly.
- Derive values during render; use effects only for external synchronization.
- Use stable unique keys.
- Extract reusable logic into custom hooks.

## Checklist

- [ ] Functional component with typed props?
- [ ] Hooks called unconditionally at top level?
- [ ] Values derived rather than synced via effects?
- [ ] Stable unique keys on lists?
- [ ] Component small and single-purpose?

## Summary

We write modern **React 19**: functional components, correct hooks, composition, and derived state. Effects synchronize with external systems only. These patterns eliminate the most common re-render and state bugs and keep components reusable.
