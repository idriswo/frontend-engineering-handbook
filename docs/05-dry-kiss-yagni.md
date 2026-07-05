# DRY, KISS, YAGNI

## Purpose

This chapter defines three principles that govern **how much** structure to add: DRY (Don't Repeat Yourself), KISS (Keep It Simple, Stupid), and YAGNI (You Aren't Gonna Need It). Together they balance reuse against simplicity and prevent both duplication and over-engineering.

## Why It Matters

The two failure modes of frontend code are opposite extremes: **copy-paste duplication** (a bug must be fixed in ten places) and **premature abstraction** (a config-driven framework for one use case). DRY fights the first, YAGNI fights the second, and KISS keeps solutions proportional to the problem. Applied together, they produce code that is reusable *and* simple.

## Core Concepts

**DRY** — Each piece of knowledge has a single, authoritative representation. Do not duplicate logic, types, or constants.

**KISS** — Prefer the simplest solution that solves the actual problem. Complexity must earn its place.

**YAGNI** — Do not build for imagined future requirements. Build what is needed now; add abstraction when a second real use case appears.

**The tension** — DRY says "unify," YAGNI says "wait." The rule of thumb: abstract on the *third* occurrence, not the first.

## Rules

1. Extract shared logic once it is genuinely duplicated (rule of three).
2. Prefer the simplest solution that meets current requirements.
3. Do not add configuration, options, or generality for hypothetical needs.
4. Duplication is cheaper than the wrong abstraction — prefer a little repetition over a leaky shared helper.
5. Constants, types, and validation schemas must have a single source of truth.

## Best Practices

- Centralize types and constants; import them everywhere.
- Wait for a real second/third use case before generalizing.
- When an abstraction feels forced, delete it and duplicate instead.

## Bad Examples

```tsx
// ❌ YAGNI violation: a "flexible" button nobody needs yet.
function Button({ as, variant, size, tone, elevation, ripple, glow, ...rest }: any) {
  // 200 lines handling combinations that are never used
}

// ❌ DRY violation: same validation copy-pasted in three forms.
if (!email.includes("@")) setError("Invalid email");
```

## Good Examples

```tsx
// ✅ KISS: a simple button covering current needs.
type ButtonProps = React.ButtonHTMLAttributes<HTMLButtonElement> & {
  variant?: "primary" | "secondary";
};

export function Button({ variant = "primary", className, ...props }: ButtonProps) {
  return <button className={cn(buttonStyles[variant], className)} {...props} />;
}

// ✅ DRY: one shared schema, reused across all forms.
export const emailSchema = z.string().email("Invalid email");
```

**Real World Example:** The email rule lives in one Zod schema; a change to the rule updates every form at once.

**Production Example:** The simple `Button` ships today; when a real third variant appears, you add one union member — not a rewrite.

**Explanation:** KISS kept the button small, DRY unified validation, and YAGNI avoided the unused "flexible" API.

## Common Mistakes

- Abstracting after the first duplication instead of the third.
- Building "flexible" components with dozens of unused props.
- Duplicating constants and types across features.
- Treating DRY as absolute and creating a tangled shared helper.

## AI Instructions

- Do not create abstractions for hypothetical future needs (YAGNI).
- Only extract shared code on genuine duplication (rule of three).
- Prefer the simplest solution that meets stated requirements (KISS).
- Keep constants, types, and schemas single-sourced (DRY).

## Checklist

- [ ] Is this the simplest solution to the actual problem?
- [ ] Am I abstracting real duplication, not imagined future needs?
- [ ] Do shared constants/types/schemas have one source of truth?
- [ ] Have I avoided unused configuration and options?

## Summary

DRY, KISS, and YAGNI balance reuse against simplicity. Unify real duplication, keep solutions proportional, and resist building for a future that may never come. When in doubt, prefer a little duplication over the wrong abstraction.
