# Project Architecture

## Purpose

This chapter defines the **overall architecture** of the application: how code is layered, how features are isolated, and how data flows. It ties together the folder structure, state management, and networking chapters into one coherent mental model.

## Why It Matters

Architecture is the set of decisions that are expensive to change later. A clear architecture lets a team of any size add features in parallel without stepping on each other, keeps business logic out of the UI, and makes the system testable. A missing architecture produces a "big ball of mud" where every change risks breaking something unrelated.

## Core Concepts

**Feature-Based Architecture.** Code is organized by feature (`features/auth`, `features/users`), not by technical type. Each feature is self-contained: components, hooks, API, and types live together.

**Layered separation.** Three conceptual layers: **UI** (presentational components), **application/state** (hooks, Redux slices, RTK Query), and **infrastructure** (Axios instance, config). UI never talks to infrastructure directly.

**Unidirectional data flow.** Data flows down through props; events flow up through callbacks; server state is owned by RTK Query.

**Shared vs. feature code.** Truly generic code lives in `shared/` or `components/ui`; feature-specific code stays inside its feature.

## Rules

1. Organize by feature, not by file type.
2. Each feature is self-contained and exposes a clear public surface via its index.
3. UI components never call Axios or `fetch` directly — they use hooks/RTK Query.
4. Business logic lives in hooks and services, not in components.
5. Cross-feature imports go through a feature's public entry point, not deep internal files.
6. Shared, generic code is promoted to `shared/` only when reused by 2+ features.

## Best Practices

- Keep features independent so they can be developed and tested in isolation.
- Define a single API layer per feature (`usersApi.ts`).
- Co-locate types with the feature that owns them.
- Promote code to `shared/` only when duplication is real.

## Bad Examples

```
❌ Organized by type — one feature scattered across the tree.
src/
  components/  (UserCard, LoginForm, ProductList...)
  hooks/       (useUser, useAuth, useCart...)
  services/    (userService, authService...)
```
Changing "users" means touching many unrelated folders.

## Good Examples

```
✅ Organized by feature — everything for a feature in one place.
src/
  features/
    users/
      components/UserCard.tsx
      hooks/useUsers.ts
      usersApi.ts
      users.types.ts
      index.ts        // public surface
  shared/
    components/ui/
    lib/
  app/
    store.ts
    router.tsx
```

**Real World Example:** A new engineer assigned to "users" opens one folder and sees the whole feature.

**Production Example:** Two teams build `users` and `billing` in parallel with zero merge conflicts because features are isolated.

**Explanation:** Feature-based structure maximizes locality of behavior — the guiding principle from chapter 01.

## Common Mistakes

- Type-based folders that scatter a single feature everywhere.
- Deep-importing another feature's internal files.
- Putting fetch logic inside components.
- Promoting everything to `shared/` prematurely.

## AI Instructions

- Place new code inside the relevant feature folder.
- Expose feature APIs through an `index.ts`; import across features only via that surface.
- Never call Axios/`fetch` from a component.
- Promote to `shared/` only on real cross-feature reuse.

## Checklist

- [ ] Is the code organized by feature?
- [ ] Is each feature self-contained with a clear public surface?
- [ ] Is business logic out of UI components?
- [ ] Are cross-feature imports going through public entry points?
- [ ] Is shared code genuinely shared before promotion?

## Summary

The architecture is **feature-based, layered, and unidirectional**. Features are self-contained, UI is separated from state and infrastructure, and shared code is promoted only on real reuse. This keeps a large app parallelizable, testable, and cheap to change.

## 🔗 Related Chapters

Read next:

- 07-folder-structure.md
- 13-redux-toolkit.md
- 14-rtk-query.md
