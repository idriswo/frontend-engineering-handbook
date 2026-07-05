# Naming Conventions

## Purpose

This chapter defines the **naming conventions** for files, folders, components, variables, functions, types, and constants. Consistent naming is the cheapest form of documentation.

## Why It Matters

Names are the interface between the reader's mind and the code. Consistent naming lets engineers predict a symbol's kind and location from its name alone, makes search reliable, and lets AI generate names that fit. Inconsistent naming forces the reader to check definitions constantly.

## Core Concepts

**Casing carries meaning.** PascalCase = components/types; camelCase = variables/functions; UPPER_SNAKE = constants; kebab-case = non-component files/folders.

**Names encode role.** Hooks start with `use`, booleans with `is/has/should/can`, event handlers with `handle`, event props with `on`.

## Rules

1. Components and their files: `PascalCase` (`UserCard.tsx`).
2. Hooks: `useCamelCase` (`useAuth.ts`).
3. Variables/functions: `camelCase`.
4. Types/interfaces: `PascalCase` (no `I` prefix).
5. Constants: `UPPER_SNAKE_CASE`.
6. Non-component files/folders: `kebab-case` (`auth-utils.ts`, `use-debounce.ts` acceptable per project — keep it consistent).
7. Booleans: `is/has/should/can` prefix.
8. Event handlers: `handleX`; handler props: `onX`.

## Best Practices

- Prefer full words over abbreviations.
- Name by intent/domain, not by implementation.
- Keep component file name identical to the component name.

## Bad Examples

```tsx
// ❌ Wrong casing, unclear intent, abbreviations.
const Get_user = () => {};
interface iUser {}
const flag = true;
function click() {}
```

## Good Examples

```tsx
// ✅ Consistent, intent-revealing.
const getUser = () => {};
interface User {}
const isAuthenticated = true;
const MAX_RETRY_COUNT = 3;

function handleSubmit() {}

type ButtonProps = {
  onClick: () => void;
};
```

**Real World Example:** Seeing `useUsers` a reader instantly knows it is a hook returning users state.

**Production Example:** A lint rule enforces `use*` naming so hooks-rules are correctly applied by ESLint.

**Explanation:** Casing and prefixes let the reader classify every symbol without opening its definition.

## Common Mistakes

- `I`-prefixed interfaces (`IUser`).
- Boolean names without `is/has` (`active` vs `isActive`).
- Mismatched component and file names.
- Random casing for constants.

## AI Instructions

- Apply the casing rules strictly by symbol kind.
- Prefix hooks with `use`, booleans with `is/has/should/can`.
- Name handlers `handleX`, handler props `onX`.
- Match component name to file name.

## Checklist

- [ ] Correct casing for each symbol kind?
- [ ] Hooks prefixed with `use`?
- [ ] Booleans prefixed correctly?
- [ ] Component name matches file name?
- [ ] No abbreviations or `I` prefixes?

## Summary

Naming conventions make code self-describing: casing signals the symbol kind, prefixes signal role, and full words signal intent. Consistency here pays off on every read, search, and AI generation.

## 🔗 Related Chapters

Read next:

- 03-clean-code.md
- 07-folder-structure.md
- 10-typescript.md
