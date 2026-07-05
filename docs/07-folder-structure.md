# Folder Structure

## Purpose

This chapter defines the **canonical folder structure** of the project. A predictable layout means any engineer or AI can locate and place code without guessing.

## Why It Matters

Folder structure is the physical expression of the architecture (chapter 06). A consistent structure removes "where does this go?" friction, prevents duplicate implementations, and makes automated tooling and AI assistants reliable. Inconsistent structure is a silent tax paid on every single change.

## Core Concepts

**Feature folders** own everything for one domain. **Shared** holds cross-cutting generic code. **App** holds composition root: store, router, providers. **Public surface** — each feature exposes an `index.ts`.

## Rules

1. Every feature lives under `src/features/<feature>/`.
2. Reusable UI primitives live under `src/shared/components/ui/`.
3. App-level wiring (store, router, providers) lives under `src/app/`.
4. Each feature exposes only what others need via `index.ts`.
5. File names follow the naming conventions in chapter 08.
6. No file lives at the root of `src/` except `main.tsx` and `App.tsx`.

## Best Practices

- Co-locate tests next to the code they test (`UserCard.test.tsx`).
- Keep folder depth shallow; avoid more than 3–4 levels.
- Group by feature first, by type second.

## Bad Examples

```
❌ Flat, typeless, ambiguous.
src/
  utils.ts
  api.ts
  Component1.tsx
  Component2.tsx
  helpers2.ts
```

## Good Examples

```
✅ Canonical structure.
src/
  app/
    store.ts
    router.tsx
    providers.tsx
  features/
    auth/
      components/
      hooks/
      authApi.ts
      auth.types.ts
      index.ts
    users/
      components/
      hooks/
      usersApi.ts
      users.types.ts
      index.ts
  shared/
    components/ui/
    hooks/
    lib/
    constants/
  main.tsx
  App.tsx
```

**Real World Example:** Adding a "notifications" feature means creating `features/notifications/` mirroring the pattern.

**Production Example:** A lint rule can forbid deep imports (`features/*/components/*`) because every feature has an `index.ts`.

**Explanation:** The structure is fractal — every feature looks the same, so navigation is muscle memory.

## Common Mistakes

- Dumping files at the `src/` root.
- Mixing feature-specific code into `shared/`.
- Inconsistent folder names across features.
- Deep nesting that hides code.

## AI Instructions

- Place new files in the correct feature or shared folder.
- Mirror the existing feature structure exactly.
- Create an `index.ts` for every new feature.
- Never create ad-hoc top-level folders.

## Checklist

- [ ] Is the file in the correct feature/shared/app location?
- [ ] Does the feature expose an `index.ts`?
- [ ] Are names consistent with other features?
- [ ] Is nesting shallow?

## Summary

A canonical, fractal folder structure — features, shared, app — makes the codebase predictable and navigable. Every feature mirrors the same layout and exposes a public `index.ts`, eliminating the "where does this go?" question.
