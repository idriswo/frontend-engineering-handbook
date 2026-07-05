# Components Example

A reusable, typed, accessible UI component. Maps to [09-react.md](../../docs/09-react.md) and the [component template](../../templates/component-template.md).

## Scenario

A `Badge` component with variants, used to show statuses across the app.

## Code

```tsx
// src/components/ui/Badge/Badge.tsx
import { type HTMLAttributes } from "react";
import { cn } from "@/lib/utils";

type Status = "success" | "warning" | "error" | "neutral";

interface BadgeProps extends HTMLAttributes<HTMLSpanElement> {
  status?: Status;
}

const styles: Record<Status, string> = {
  success: "bg-green-100 text-green-800",
  warning: "bg-yellow-100 text-yellow-800",
  error: "bg-red-100 text-red-800",
  neutral: "bg-gray-100 text-gray-800",
};

export function Badge({ status = "neutral", className, ...props }: BadgeProps) {
  return (
    <span
      className={cn("inline-flex rounded-full px-2 py-0.5 text-xs font-medium", styles[status], className)}
      {...props}
    />
  );
}
```

```tsx
// Usage
<Badge status="success">Active</Badge>
<Badge status="error">Failed</Badge>
```

## Explanation

- Extends `HTMLAttributes` so it behaves like a native `<span>`.
- A `Record<Status, string>` maps variants to classes — no `if/else` chains.
- `cn()` merges caller `className`, keeping the component overridable.

## Related

[Component template](../../templates/component-template.md) · [08-naming-conventions.md](../../docs/08-naming-conventions.md)
