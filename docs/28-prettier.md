# Prettier

## Purpose

This chapter defines our **Prettier** setup: automatic, opinionated code formatting that ends all style debates and keeps diffs clean.

## Why It Matters

Formatting debates waste time and pollute diffs with noise. Prettier makes formatting a solved, automatic problem — every file looks the same regardless of who (or which AI) wrote it. This shrinks diffs to meaningful changes and removes formatting from code review entirely.

## Core Concepts

**Opinionated & automatic.** Prettier reformats on save/commit; engineers don't hand-format.

**Single config.** One `.prettierrc` is the source of formatting truth.

**Separation from ESLint.** Prettier handles formatting; ESLint handles correctness. They don't overlap.

**Tailwind ordering.** The Prettier Tailwind plugin sorts class names consistently.

## Rules

1. All code is formatted by Prettier; no manual formatting.
2. One shared config; no per-developer overrides.
3. Format on save and enforce via pre-commit (chapter 29).
4. Keep Prettier and ESLint responsibilities separate (use `eslint-config-prettier` to avoid conflicts).
5. Use the Tailwind plugin for consistent class ordering.

## Best Practices

- Enable format-on-save in the editor.
- Run `prettier --check` in CI.
- Commit the config so every environment matches.

## Bad Examples

```tsx
// ❌ Hand-formatted, inconsistent, will churn diffs.
const x = {a:1,b:2,
    c:3}
function f( a,b ){return a+b}
```

## Good Examples

```jsonc
// .prettierrc
{
  "semi": true,
  "singleQuote": false,
  "trailingComma": "all",
  "printWidth": 100,
  "plugins": ["prettier-plugin-tailwindcss"]
}
```

```tsx
// ✅ Prettier-formatted, consistent, minimal diffs.
const point = { a: 1, b: 2, c: 3 };

function add(a: number, b: number): number {
  return a + b;
}
```

**Real World Example:** Two engineers editing the same file produce identical formatting, so diffs show only real changes.

**Production Example:** The Tailwind plugin reorders classes deterministically, preventing class-order churn in PRs.

**Explanation:** Automatic formatting removes style from the human loop entirely.

## Common Mistakes

- Hand-formatting and fighting Prettier.
- Overlapping ESLint formatting rules causing conflicts.
- Per-developer config differences.
- Not running the check in CI.

## AI Instructions

- Output code that matches the Prettier config.
- Never hand-format against Prettier.
- Assume format-on-commit; keep formatting out of logic diffs.
- Respect Tailwind class-ordering.

## Checklist

- [ ] Is the code Prettier-formatted?
- [ ] Is there a single shared config?
- [ ] Are ESLint/Prettier responsibilities separate?
- [ ] Is formatting enforced in CI/pre-commit?

## Summary

Prettier makes formatting automatic and uniform. One shared config, format-on-save, and pre-commit enforcement eliminate style debates and keep diffs meaningful — with ESLint owning correctness and Prettier owning style.

## 🔗 Related Chapters

Read next:

- 27-eslint.md
- 29-husky.md
- 08-naming-conventions.md
