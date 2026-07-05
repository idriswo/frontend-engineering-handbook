# Lucide React

## Purpose

This chapter defines how we use **Lucide React** for icons: consistent sizing, accessibility, tree-shaking, and integration with our components.

## Why It Matters

Icons are everywhere and easy to get wrong: inconsistent sizes, missing accessible labels, and bloated bundles from importing whole icon sets. Lucide React gives a consistent, tree-shakable, customizable icon system. A clear convention keeps icons visually uniform and accessible.

## Core Concepts

**Named imports.** Import only the icons used so bundlers tree-shake the rest.

**Sizing via props/tokens.** Control size with `size`/Tailwind classes, not hard-coded styles.

**Accessibility.** Decorative icons are hidden from assistive tech; meaningful icons get labels.

**`currentColor`.** Icons inherit text color by default, integrating with theme tokens.

## Rules

1. Import icons by name (`import { Search } from "lucide-react"`) — never namespace-import the whole set.
2. Decorative icons must be `aria-hidden`; icon-only controls need an accessible label.
3. Size via `className`/`size` prop using consistent tokens.
4. Let icons inherit `currentColor` rather than hard-coding colors.
5. Wrap icon-only buttons with accessible text (`aria-label` or visually-hidden text).

## Best Practices

- Standardize default icon sizes (e.g., 16/20/24) via component wrappers.
- Co-locate icon choices with the component, not a giant icon map.
- Keep stroke width consistent with the design system.

## Bad Examples

```tsx
// ❌ Icon-only button with no label; hard-coded color; whole-set import.
import * as Icons from "lucide-react";
<button><Icons.Trash2 color="#ff0000" /></button>
```

## Good Examples

```tsx
import { Trash2, Search } from "lucide-react";

// ✅ Decorative icon hidden; inherits color and size from context.
function SearchField() {
  return (
    <div className="flex items-center gap-2 text-muted-foreground">
      <Search aria-hidden className="h-4 w-4" />
      <input aria-label="Search" className="bg-transparent" />
    </div>
  );
}

// ✅ Icon-only button with an accessible label.
function DeleteButton({ onDelete }: { onDelete: () => void }) {
  return (
    <button onClick={onDelete} aria-label="Delete item">
      <Trash2 aria-hidden className="h-4 w-4" />
    </button>
  );
}
```

**Real World Example:** Named imports keep only `Search` and `Trash2` in the bundle.

**Production Example:** The delete button announces "Delete item" to screen readers despite showing only an icon.

**Explanation:** Named imports + `currentColor` + accessible labels give small, consistent, inclusive icons.

## Common Mistakes

- Namespace-importing the whole icon set (bundle bloat).
- Icon-only buttons without accessible labels.
- Hard-coded icon colors instead of `currentColor`.
- Inconsistent icon sizes across the app.

## AI Instructions

- Import icons by name only.
- Mark decorative icons `aria-hidden`; label icon-only controls.
- Size via className/tokens; let color inherit.
- Keep sizes consistent with the design system.

## Checklist

- [ ] Icons imported by name (tree-shakable)?
- [ ] Decorative icons `aria-hidden`?
- [ ] Icon-only controls labelled?
- [ ] Sizing and color consistent with tokens?

## Summary

Lucide React provides consistent, tree-shakable icons. We import by name, size via tokens, let icons inherit `currentColor`, hide decorative icons from assistive tech, and label icon-only controls — keeping icons small, uniform, and accessible.

## 🔗 Related Chapters

Read next:

- 20-tailwind-css.md
- 21-shadcn-ui.md
- 22-framer-motion.md
