# Tailwind CSS

## Purpose

This chapter defines how we style with **Tailwind CSS**: utility-first styling, design tokens, conditional classes with `clsx`/`tailwind-merge`, and responsive/dark-mode patterns.

## Why It Matters

Tailwind keeps styling co-located with markup, eliminates dead CSS, and enforces a design system through tokens. Consistent Tailwind usage prevents the "one-off magic values everywhere" problem and makes components visually consistent. Misused, it produces unreadable class soup and duplicated conditional logic.

## Core Concepts

**Utility-first.** Compose styles from small utilities rather than hand-written CSS.

**Design tokens.** Colors, spacing, and typography come from the Tailwind theme, not arbitrary values.

**`clsx` + `tailwind-merge`.** Combine conditional classes (`clsx`) and resolve conflicts (`tailwind-merge`) via a `cn()` helper.

**Responsive & dark mode.** Use `sm:`/`md:` breakpoints and `dark:` variants.

## Rules

1. Use Tailwind utilities; no inline `style` and no ad-hoc CSS files for component styling.
2. Use theme tokens (`text-primary`, `p-4`) — avoid arbitrary values (`p-[13px]`) unless justified.
3. Merge conditional classes with a `cn()` helper (`clsx` + `tailwind-merge`).
4. Support dark mode via `dark:` variants where the design requires it.
5. Extract repeated class sets into components, not copy-pasted strings.

## Best Practices

- Define brand tokens in `tailwind.config`.
- Keep class lists ordered/consistent (use the Prettier Tailwind plugin).
- Prefer semantic component wrappers over repeating long class lists.

## Bad Examples

```tsx
// ❌ Inline styles, arbitrary values, manual conditional string concat.
<div style={{ padding: 13 }} className={"p-[13px] " + (active ? "bg-blue-500" : "")}>
```

## Good Examples

```ts
// shared/lib/cn.ts
import { clsx, type ClassValue } from "clsx";
import { twMerge } from "tailwind-merge";

export const cn = (...inputs: ClassValue[]) => twMerge(clsx(inputs));
```

```tsx
// ✅ Tokens, cn() for conditionals, no inline styles.
function Badge({ active, className }: { active: boolean; className?: string }) {
  return (
    <span
      className={cn(
        "rounded-full px-3 py-1 text-sm",
        active ? "bg-primary text-primary-foreground" : "bg-muted text-muted-foreground",
        className
      )}
    />
  );
}
```

**Real World Example:** `cn()` lets a parent override `Badge` styles without conflicting class duplication.

**Production Example:** `tailwind-merge` ensures a passed `px-4` overrides the default `px-3` instead of both landing in the DOM.

**Explanation:** Tokens + `cn()` give consistent, conflict-free, overridable styling.

## Common Mistakes

- Inline styles and arbitrary magic values.
- String-concatenating conditional classes (no conflict resolution).
- Copy-pasting long class lists instead of extracting components.
- Ignoring dark mode where the design requires it.

## AI Instructions

- Style with Tailwind utilities and theme tokens.
- Use the `cn()` helper for conditional/merged classes.
- No inline styles or arbitrary values without justification.
- Add `dark:` variants where dark mode applies.

## Checklist

- [ ] Utilities and tokens used (no inline styles)?
- [ ] Conditional classes handled with `cn()`?
- [ ] Repeated class sets extracted into components?
- [ ] Dark mode supported where required?

## Summary

Tailwind gives utility-first, token-driven styling co-located with markup. We compose classes with a `cn()` helper (`clsx` + `tailwind-merge`), rely on design tokens, and support responsive and dark-mode variants — keeping styling consistent and conflict-free.

## 🔗 Related Chapters

Read next:

- 21-shadcn-ui.md
- 22-framer-motion.md
- 23-lucide-react.md
