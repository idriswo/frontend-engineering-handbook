# shadcn/ui

## Purpose

This chapter defines how we use **shadcn/ui**: copy-in, composable, accessible components built on Radix primitives and Tailwind. It is our component foundation for buttons, dialogs, forms, and more.

## Why It Matters

shadcn/ui is not a dependency-locked library — components are copied into the codebase and owned by us. This gives full control, accessibility out of the box (Radix), and Tailwind-native styling. Used consistently it prevents reinventing accessible primitives; misused, it becomes a pile of un-customized, inconsistent copies.

## Core Concepts

**Owned components.** Components live in `shared/components/ui/` and are edited like our own code.

**Radix underneath.** Accessibility (focus, keyboard, ARIA) comes from Radix primitives.

**Composition + `cn()`.** Variants via `class-variance-authority` (cva) and the `cn()` helper.

**Consistent theming.** Components read design tokens, so theme changes propagate everywhere.

## Rules

1. Use shadcn/ui components for common primitives instead of building from scratch.
2. Keep generated components in `shared/components/ui/`; customize them there.
3. Preserve accessibility — do not strip Radix behavior or ARIA.
4. Extend variants with `cva`, not by forking components.
5. Compose shadcn primitives into feature components; don't scatter raw usage.

## Best Practices

- Wrap frequently-customized primitives in a project component (`AppButton`) if you need consistent defaults.
- Keep component APIs prop-driven and typed.
- Integrate form primitives with React Hook Form via `Controller`.

## Bad Examples

```tsx
// ❌ Reinventing an accessible dialog by hand — no focus trap, no ARIA.
{open && (
  <div className="fixed inset-0" onClick={close}>
    <div>{children}</div>
  </div>
)}
```

## Good Examples

```tsx
// Using the owned shadcn Dialog primitive (accessible via Radix).
import {
  Dialog,
  DialogContent,
  DialogHeader,
  DialogTitle,
  DialogTrigger,
} from "@/shared/components/ui/dialog";
import { Button } from "@/shared/components/ui/button";

export function ConfirmDelete({ onConfirm }: { onConfirm: () => void }) {
  return (
    <Dialog>
      <DialogTrigger asChild>
        <Button variant="destructive">Delete</Button>
      </DialogTrigger>
      <DialogContent>
        <DialogHeader>
          <DialogTitle>Delete item?</DialogTitle>
        </DialogHeader>
        <Button variant="destructive" onClick={onConfirm}>
          Confirm
        </Button>
      </DialogContent>
    </Dialog>
  );
}
```

**Real World Example:** The dialog traps focus, closes on Escape, and is screen-reader labelled — all for free.

**Production Example:** A theme token change restyles every shadcn component at once, keeping the UI consistent.

**Explanation:** Owning accessible primitives lets us customize freely without losing correctness.

## Common Mistakes

- Hand-building primitives that Radix already solves accessibly.
- Forking components instead of extending variants with `cva`.
- Scattering raw primitive usage instead of composing feature components.
- Overriding styles in ways that break accessibility (e.g., removing focus rings).

## AI Instructions

- Use shadcn/ui primitives for common components.
- Keep and customize them in `shared/components/ui/`.
- Preserve Radix accessibility; extend via `cva` variants.
- Integrate form controls with React Hook Form.

## Checklist

- [ ] Are accessible primitives reused instead of rebuilt?
- [ ] Are components owned in `shared/components/ui/`?
- [ ] Is accessibility preserved (focus, ARIA, keyboard)?
- [ ] Are variants added via `cva`, not forks?

## Summary

shadcn/ui gives us owned, accessible, Tailwind-native components built on Radix. We keep them in `shared/components/ui/`, extend them with `cva` variants, preserve their accessibility, and compose them into feature components.
