# ESLint

## Purpose

This chapter defines our **ESLint** setup: the rules that enforce correctness and consistency automatically, and how engineers and AI must respect them.

## Why It Matters

ESLint catches bugs and enforces conventions before code review, making standards automatic rather than a matter of memory or debate. It removes an entire category of review comments and keeps the codebase consistent across contributors and AI. Disabling rules casually erodes this safety net.

## Core Concepts

**Correctness rules.** Catch real bugs (Rules of Hooks, unused vars, no-floating-promises).

**Consistency rules.** Enforce conventions (import order, naming) so all code looks the same.

**TypeScript-aware linting.** `typescript-eslint` adds type-informed rules.

**No arbitrary disables.** Suppressions require a reason.

## Rules

1. Code must pass ESLint with zero errors before commit (enforced by Husky, chapter 29).
2. Follow the React Hooks rules — no exceptions.
3. Do not disable rules inline without a justifying comment.
4. Fix the cause of a lint error, not the symptom (no blanket disables).
5. Keep the config as the single source of lint truth; no per-file overrides without reason.

## Best Practices

- Enable `eslint-plugin-react-hooks` and `@typescript-eslint` recommended rules.
- Integrate ESLint with the editor for instant feedback.
- Run lint in CI as a hard gate.

## Bad Examples

```tsx
// ❌ Blanket disable to silence a real bug.
/* eslint-disable */
useEffect(() => { fetchData(); }); // missing deps hidden by disable
```

## Good Examples

```jsonc
// eslint.config.js (flat config, simplified)
export default [
  js.configs.recommended,
  ...tseslint.configs.recommendedTypeChecked,
  {
    plugins: { "react-hooks": reactHooks },
    rules: {
      "react-hooks/rules-of-hooks": "error",
      "react-hooks/exhaustive-deps": "warn",
      "@typescript-eslint/no-explicit-any": "error",
      "@typescript-eslint/no-floating-promises": "error",
    },
  },
];
```

```tsx
// ✅ Fix the cause; if a disable is truly needed, justify it.
// eslint-disable-next-line react-hooks/exhaustive-deps -- runs once on mount by design
useEffect(() => { initAnalytics(); }, []);
```

**Real World Example:** `no-explicit-any` as an error stops untyped code from ever merging.

**Production Example:** `no-floating-promises` catches an un-awaited async call that would have silently dropped an error.

**Explanation:** Enforced, type-aware rules turn conventions into an automatic gate, not a review chore.

## Common Mistakes

- Blanket `/* eslint-disable */` to silence real issues.
- Ignoring hook dependency warnings.
- Editor lint disabled, so problems surface only in CI.
- Per-file overrides that weaken the shared config.

## AI Instructions

- Generate code that passes the ESLint config with zero errors.
- Never add blanket disables; justify any single-line disable.
- Respect the Rules of Hooks and dependency arrays.
- Fix the root cause of lint errors.

## Checklist

- [ ] Does the code pass ESLint with zero errors?
- [ ] Are there any unjustified inline disables?
- [ ] Are hook rules and deps respected?
- [ ] Is `any` avoided?

## Summary

ESLint makes our standards automatic: type-aware correctness and consistency rules gate every commit and CI run. We fix causes, never blanket-disable rules, and treat a clean lint pass as a hard requirement.
