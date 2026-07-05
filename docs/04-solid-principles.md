# SOLID Principles

## Purpose

This chapter adapts the five **SOLID** principles to a modern React + TypeScript frontend. SOLID originated in object-oriented design, but its intent — decoupled, changeable, testable code — applies directly to components, hooks, and modules.

## Why It Matters

Frontends grow fast and change constantly. Without design discipline, components accumulate responsibilities, become impossible to test, and break in unexpected ways when touched. SOLID gives a shared vocabulary for keeping units small, focused, and replaceable, so change stays cheap as the app scales.

## Core Concepts

**S — Single Responsibility.** A component/hook/module has one reason to change. UI, data fetching, and business logic are separated.

**O — Open/Closed.** Units are open for extension, closed for modification — extend via props, composition, and configuration, not by editing internals.

**L — Liskov Substitution.** A component must be replaceable by another honoring the same props contract without breaking consumers.

**I — Interface Segregation.** Prefer small, focused prop interfaces over large ones forcing consumers to depend on props they don't use.

**D — Dependency Inversion.** High-level components depend on abstractions (hooks, context, injected functions), not concrete implementations.

## Rules

1. A component renders UI; it does not fetch data or hold business logic directly — delegate to hooks and services.
2. Extend behavior through props and composition, never by editing shared internals.
3. Keep prop interfaces minimal and cohesive.
4. Depend on injected abstractions (hooks/context) so implementations can be swapped or mocked.
5. Any variant component must honor the base props contract.

## Best Practices

- Extract data logic into custom hooks (`useUsers`) and UI into presentational components.
- Use composition (`children`, render props, slots) to keep components open/closed.
- Inject side-effecting dependencies so tests can substitute mocks.

## Bad Examples

```tsx
// ❌ One component fetches, transforms, and renders — many reasons to change.
function Dashboard() {
  const [users, setUsers] = useState<any[]>([]);
  useEffect(() => {
    axios.get("/users").then((r) => setUsers(r.data.filter((u: any) => u.active)));
  }, []);
  return <div>{users.map((u) => <span key={u.id}>{u.name}</span>)}</div>;
}
```

## Good Examples

```tsx
// ✅ SRP + Dependency Inversion: data in a hook, UI is pure.
function useActiveUsers() {
  const { data } = useGetUsersQuery();
  return (data ?? []).filter((user) => user.isActive);
}

function UserBadges({ users }: { users: User[] }) {
  return (
    <div>
      {users.map((user) => (
        <span key={user.id}>{user.name}</span>
      ))}
    </div>
  );
}

export function Dashboard() {
  const activeUsers = useActiveUsers();
  return <UserBadges users={activeUsers} />;
}
```

**Real World Example:** `UserBadges` can be reused in a sidebar, modal, or report — it only depends on a `users` prop.

**Production Example:** When the API changes, only `useActiveUsers` changes; the UI and its tests stay untouched.

**Explanation:** Separating concerns makes each unit independently testable and reusable — the core payoff of SOLID.

## Common Mistakes

- "God components" that fetch, transform, and render.
- Adding `if (variant === "x")` branches instead of composing (violates Open/Closed).
- Huge prop interfaces where most props are optional and unrelated.
- Importing concrete services directly into UI, making tests require real network.

## AI Instructions

- Separate data-fetching (hooks) from rendering (presentational components).
- Extend via props/composition, never by editing shared internals.
- Keep prop interfaces small and cohesive.
- Inject dependencies so units are testable in isolation.

## Checklist

- [ ] Does each unit have a single reason to change?
- [ ] Can behavior be extended without editing internals?
- [ ] Are prop interfaces small and focused?
- [ ] Do components depend on abstractions, not concretions?
- [ ] Can each unit be tested in isolation?

## Summary

SOLID keeps a growing frontend maintainable by enforcing focused, decoupled, replaceable units. In React this means: data in hooks, UI in presentational components, extension via composition, minimal prop contracts, and injected dependencies.
