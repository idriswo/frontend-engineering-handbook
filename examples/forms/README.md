# Forms Example

A validated form with React Hook Form + Zod. Maps to [18-react-hook-form.md](../../docs/18-react-hook-form.md) and [19-zod.md](../../docs/19-zod.md).

## Scenario

A login form with typed, schema-validated fields.

## Code

```tsx
// src/features/auth/components/LoginForm.tsx
import { useForm } from "react-hook-form";
import { zodResolver } from "@hookform/resolvers/zod";
import { z } from "zod";

const loginSchema = z.object({
  email: z.string().email("Enter a valid email"),
  password: z.string().min(8, "At least 8 characters"),
});

type LoginValues = z.infer<typeof loginSchema>;

export function LoginForm({ onSubmit }: { onSubmit: (values: LoginValues) => void }) {
  const {
    register,
    handleSubmit,
    formState: { errors, isSubmitting },
  } = useForm<LoginValues>({ resolver: zodResolver(loginSchema) });

  return (
    <form onSubmit={handleSubmit(onSubmit)} className="space-y-4" noValidate>
      <div>
        <input type="email" placeholder="Email" {...register("email")} />
        {errors.email && <p className="text-sm text-red-600">{errors.email.message}</p>}
      </div>

      <div>
        <input type="password" placeholder="Password" {...register("password")} />
        {errors.password && <p className="text-sm text-red-600">{errors.password.message}</p>}
      </div>

      <button type="submit" disabled={isSubmitting}>
        {isSubmitting ? "Signing in…" : "Sign in"}
      </button>
    </form>
  );
}
```

## Explanation

- The Zod schema is the **single source of truth**; `z.infer` derives the TS type — no duplicate typing.
- `zodResolver` wires validation into RHF; errors render inline.
- `isSubmitting` disables the button to prevent double submits.

## Related

[19-zod.md](../../docs/19-zod.md) · [18-react-hook-form.md](../../docs/18-react-hook-form.md)
