# Component Template

A reusable, typed presentational component. Copy, rename, and adapt.
See [09-react.md](../docs/09-react.md) and [08-naming-conventions.md](../docs/08-naming-conventions.md).

## Structure

```
Button/
├── Button.tsx        # Component + typed props
├── Button.test.tsx   # Unit tests
└── index.ts          # Public re-export
```

## Template

```tsx
// Button.tsx
import { type ButtonHTMLAttributes, forwardRef } from "react";
import { cn } from "@/lib/utils";

type Variant = "primary" | "secondary" | "ghost";

interface ButtonProps extends ButtonHTMLAttributes<HTMLButtonElement> {
  variant?: Variant;
  isLoading?: boolean;
}

const variantClasses: Record<Variant, string> = {
  primary: "bg-primary text-primary-foreground hover:bg-primary/90",
  secondary: "bg-secondary text-secondary-foreground hover:bg-secondary/80",
  ghost: "bg-transparent hover:bg-accent",
};

export const Button = forwardRef<HTMLButtonElement, ButtonProps>(
  ({ variant = "primary", isLoading = false, className, children, disabled, ...props }, ref) => {
    return (
      <button
        ref={ref}
        disabled={disabled || isLoading}
        className={cn(
          "inline-flex items-center justify-center rounded-md px-4 py-2 text-sm font-medium transition-colors disabled:opacity-50",
          variantClasses[variant],
          className,
        )}
        {...props}
      >
        {isLoading ? "Loading…" : children}
      </button>
    );
  },
);

Button.displayName = "Button";
```

```ts
// index.ts
export { Button } from "./Button";
```

## Rules

- Typed props via an `interface`; extend native element props when wrapping HTML elements.
- One component per file; export from `index.ts`.
- No business logic or data fetching inside presentational components — pass data and callbacks via props.
- Compose styles with `cn()` so callers can override with `className`.
- Forward refs for form/DOM interoperability.

## Checklist

- [ ] Props typed, defaults set
- [ ] `className` merged, not overwritten
- [ ] No side effects / data fetching inside
- [ ] Accessible (semantic element, `aria-*` where needed)
- [ ] Re-exported from `index.ts`
