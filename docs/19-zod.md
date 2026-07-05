# Zod

## Purpose

This chapter defines how we use **Zod** for runtime validation and type inference: schema definition, integration with forms, and validating external data (API responses, env, params).

## Why It Matters

TypeScript types vanish at runtime. Data crossing the boundary — API responses, form input, URL params, env variables — is `unknown` and can violate its declared type. Zod validates at runtime and *infers* the static type from the same schema, giving one source of truth for both compile-time and runtime safety.

## Core Concepts

**Schema = type + validator.** One Zod schema produces both a runtime validator and a static type via `z.infer`.

**Parse vs. safeParse.** `parse` throws; `safeParse` returns a typed result object.

**Boundary validation.** Validate all external/untrusted data at the edge.

**Composition.** Schemas compose (`extend`, `merge`, `pick`, `omit`) like types.

## Rules

1. Define one schema per entity; infer the type — never hand-write a parallel `interface`.
2. Validate all external data at the boundary (API, forms, env, params).
3. Use `safeParse` where failure is expected/handled; `parse` where failure is exceptional.
4. Provide clear, user-facing error messages in schema definitions.
5. Reuse and compose schemas instead of duplicating fields.

## Best Practices

- Co-locate schemas with the feature's types.
- Share primitive schemas (`emailSchema`, `idSchema`) across features.
- Validate API responses in the RTK Query `transformResponse` when correctness matters.

## Bad Examples

```tsx
// ❌ Duplicated type and validator, drift guaranteed, trusts raw data.
interface User { id: string; email: string; }
const user = (await api.get("/me")).data as User; // no runtime check
```

## Good Examples

```ts
import { z } from "zod";

export const userSchema = z.object({
  id: z.string().uuid(),
  email: z.string().email("Invalid email"),
  roles: z.array(z.enum(["admin", "user"])),
});

export type User = z.infer<typeof userSchema>;

// Boundary validation of an API response.
export function parseUser(data: unknown): User {
  return userSchema.parse(data);
}

// Expected-failure path.
const result = userSchema.safeParse(input);
if (!result.success) {
  showValidationErrors(result.error.flatten());
}
```

**Real World Example:** `User` is inferred from `userSchema`, so the type and the runtime check can never disagree.

**Production Example:** A backend that suddenly returns `roles: null` fails validation at the boundary instead of crashing deep in the UI.

**Explanation:** One schema secures both the compile-time type and the runtime shape — the core value of Zod.

## Common Mistakes

- Writing a separate `interface` alongside the schema.
- Casting API data with `as` instead of parsing.
- Using `parse` where errors are expected (should be `safeParse`).
- Vague error messages.

## AI Instructions

- Define schemas and infer types; never duplicate with `interface`.
- Validate external/untrusted data at the boundary.
- Choose `parse` vs `safeParse` by whether failure is exceptional or expected.
- Write clear validation messages.

## Checklist

- [ ] Is the type inferred from the schema (not duplicated)?
- [ ] Is external data validated at the boundary?
- [ ] Is `parse`/`safeParse` chosen correctly?
- [ ] Are error messages clear and user-facing?

## Summary

Zod unifies runtime validation and static typing: one schema yields both the validator and the type. We validate all boundary data, infer types with `z.infer`, and choose `parse`/`safeParse` by intent — closing the gap TypeScript leaves at runtime.

## 🔗 Related Chapters

Read next:

- 18-react-hook-form.md
- 10-typescript.md
- 14-rtk-query.md
