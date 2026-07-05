# React Hook Form

## Purpose

This chapter defines how we build forms with **React Hook Form (RHF)**: registration, validation via Zod resolvers, submission, and error handling. RHF is the only approved way to build forms.

## Why It Matters

Forms are where most user input bugs live. RHF gives performant, uncontrolled-by-default forms with minimal re-renders, built-in validation integration, and clean error handling. Building forms with raw `useState` reintroduces re-render storms, manual validation, and inconsistent UX.

## Core Concepts

**Uncontrolled by default.** RHF tracks inputs via refs, minimizing re-renders.

**Resolver validation.** Zod schemas validate through `@hookform/resolvers/zod` (chapter 19).

**`handleSubmit`.** Runs validation, then your typed submit handler.

**Typed forms.** The form values type is inferred from the Zod schema.

## Rules

1. Use React Hook Form for all forms — never `useState` per field.
2. Validate with a Zod schema via `zodResolver`.
3. Infer the form values type from the schema (single source of truth).
4. Display field errors accessibly (associate messages with inputs).
5. Disable submit while submitting; handle server errors.

## Best Practices

- Keep the schema next to the form or in the feature's types.
- Use `Controller` for controlled UI libraries (e.g., shadcn/ui inputs).
- Reset the form on successful submit when appropriate.

## Bad Examples

```tsx
// ❌ useState per field, manual validation, re-render on every keystroke.
const [email, setEmail] = useState("");
const [error, setError] = useState("");
const submit = () => {
  if (!email.includes("@")) setError("Invalid");
};
```

## Good Examples

```tsx
import { useForm } from "react-hook-form";
import { zodResolver } from "@hookform/resolvers/zod";
import { z } from "zod";

const loginSchema = z.object({
  email: z.string().email("Invalid email"),
  password: z.string().min(8, "Min 8 characters"),
});

type LoginValues = z.infer<typeof loginSchema>;

export function LoginForm({ onLogin }: { onLogin: (v: LoginValues) => Promise<void> }) {
  const {
    register,
    handleSubmit,
    formState: { errors, isSubmitting },
  } = useForm<LoginValues>({ resolver: zodResolver(loginSchema) });

  return (
    <form onSubmit={handleSubmit(onLogin)} noValidate>
      <input type="email" aria-invalid={!!errors.email} {...register("email")} />
      {errors.email && <p role="alert">{errors.email.message}</p>}

      <input type="password" aria-invalid={!!errors.password} {...register("password")} />
      {errors.password && <p role="alert">{errors.password.message}</p>}

      <button type="submit" disabled={isSubmitting}>Log in</button>
    </form>
  );
}
```

**Real World Example:** The values type comes from the schema, so validation and TypeScript never drift apart.

**Production Example:** `isSubmitting` disables the button, preventing duplicate submissions on slow networks.

**Explanation:** RHF + Zod give typed, accessible, performant forms with a single source of validation truth.

## Common Mistakes

- `useState` per field (re-render storms, manual validation).
- Duplicating the values type instead of inferring from the schema.
- Missing `aria-invalid`/`role="alert"` accessibility.
- Not disabling submit during submission.

## AI Instructions

- Build all forms with React Hook Form.
- Validate via `zodResolver` and infer the values type from the schema.
- Wire accessible error messages (`aria-invalid`, `role="alert"`).
- Disable submit while submitting.

## Checklist

- [ ] Is the form built with RHF (no per-field `useState`)?
- [ ] Is validation done with a Zod resolver?
- [ ] Is the values type inferred from the schema?
- [ ] Are errors displayed accessibly?
- [ ] Is submit disabled while submitting?

## Summary

React Hook Form is the standard for all forms: performant, uncontrolled inputs, Zod-based validation, schema-inferred types, and accessible error handling. Forms are never built with raw `useState`.
