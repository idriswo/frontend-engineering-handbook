# TypeScript

## Purpose

This chapter defines how we use **TypeScript** to make the codebase safe, self-documenting, and refactorable. TypeScript is mandatory for all code — there is no plain JavaScript in this project.

## Why It Matters

Types are executable documentation and a safety net. They catch entire classes of bugs at compile time, power editor autocomplete, and make large refactors safe. In a team and AI setting, types are the contract that keeps everyone aligned. Weak typing (`any`) throws away every one of these benefits.

## Core Concepts

**Strict mode always.** `strict: true` in `tsconfig`. No implicit `any`.

**Model the domain.** Types describe real business entities, not just shapes convenient for one call site.

**Prefer `type` for unions/utility, `interface` for object contracts** — stay consistent.

**Narrowing over casting.** Use type guards and discriminated unions instead of `as`.

**`unknown` over `any`** when a type is genuinely unknown, then narrow.

## Rules

1. `strict` mode is on; never disable it.
2. Never use `any` unless unavoidable — document why, prefer `unknown`.
3. Type all function parameters, returns, and component props.
4. Avoid `as` casts; narrow with type guards instead.
5. Use discriminated unions for state (`{ status: "loading" } | { status: "error"; error }`).
6. Share domain types from a single source; never redefine them per feature.

## Best Practices

- Derive types from a single source with utility types (`Pick`, `Omit`, `ReturnType`).
- Prefer `readonly` for data that should not mutate.
- Use `satisfies` to validate a value against a type without widening.
- Co-locate types with the feature that owns them.

## Bad Examples

```tsx
// ❌ any, casting, untyped params — no safety at all.
function save(data: any) {
  const user = data as User;
  api.post("/users", user);
}
```

## Good Examples

```tsx
// ✅ Strict types, discriminated union, guard.
type RequestState<T> =
  | { status: "idle" }
  | { status: "loading" }
  | { status: "success"; data: T }
  | { status: "error"; error: string };

interface User {
  id: string;
  name: string;
  email: string;
}

function isSuccess<T>(
  state: RequestState<T>
): state is Extract<RequestState<T>, { status: "success" }> {
  return state.status === "success";
}
```

**Real World Example:** The discriminated union makes it impossible to read `data` while `status` is `"loading"` — the compiler forbids it.

**Production Example:** Renaming `User.email` triggers compile errors at every use site, so no reference is missed during refactor.

**Explanation:** Modeling state as a union plus guards turns runtime bugs into compile-time errors.

## Common Mistakes

- Using `any` to silence errors.
- Casting with `as` instead of narrowing.
- Duplicating domain types across features.
- Optional-everything interfaces that model nothing.

## AI Instructions

- Keep strict mode; never use `any` without a documented reason.
- Type all params, returns, and props.
- Model state as discriminated unions; narrow with guards.
- Reuse shared domain types; never redefine them.

## Checklist

- [ ] Is everything typed (params, returns, props)?
- [ ] Any unjustified `any` or `as`?
- [ ] Is state modeled as a discriminated union where appropriate?
- [ ] Are domain types single-sourced?

## Summary

TypeScript in strict mode is the contract layer of the app. We model the domain precisely, avoid `any` and `as`, use discriminated unions and guards, and single-source domain types — turning whole classes of runtime bugs into compile-time errors.

## 🔗 Related Chapters

Read next:

- 09-react.md
- 14-rtk-query.md
- 19-zod.md
