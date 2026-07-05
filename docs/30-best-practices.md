# Best Practices

## Purpose

This chapter consolidates the **cross-cutting best practices** that don't belong to a single technology: environment variables, constants, helpers, reusable components, custom hooks, and theming (theme provider, dark/light mode). It is the "glue" that keeps the whole app cohesive.

## Why It Matters

Most quality problems live between the tools: a secret hard-coded here, a magic string there, a copy-pasted helper, an inconsistent theme. These small decisions compound across the codebase. Codifying them keeps the app consistent, secure, and maintainable regardless of which feature you're in.

## Core Concepts

**Environment variables.** All config via typed `VITE_` env, validated at startup (chapter 11).

**Constants.** No magic values — named constants with a single source of truth.

**Helpers & utils.** Small, pure, well-named, and shared only when genuinely reused.

**Reusable components & hooks.** Extract on real duplication; keep APIs typed and minimal.

**Theming.** A theme provider drives dark/light mode via tokens, respecting system preference.

## Rules

1. All configuration comes from typed environment variables; no hard-coded URLs/keys.
2. Extract magic numbers/strings into named constants with one source of truth.
3. Helpers are pure, typed, and single-purpose; shared only on real reuse.
4. Reusable components/hooks are extracted on the rule of three, not speculatively.
5. Theming is centralized in a theme provider using design tokens.
6. Respect the user's system color-scheme preference by default.

## Best Practices

- Keep a `constants/` module per feature and a shared one for global constants.
- Persist theme choice and sync with `prefers-color-scheme`.
- Prefer composition and hooks over utility grab-bags.

## Bad Examples

```tsx
// ❌ Hard-coded URL, magic string, ad-hoc theme toggle.
fetch("https://api.prod.example.com/v1/users");
if (role === "adm") { /* ... */ }
document.body.style.background = dark ? "#000" : "#fff";
```

## Good Examples

```ts
// shared/constants/roles.ts
export const ROLES = { admin: "admin", user: "user" } as const;
export type Role = (typeof ROLES)[keyof typeof ROLES];
```

```tsx
// app/ThemeProvider.tsx — centralized theming with system preference.
export function ThemeProvider({ children }: { children: React.ReactNode }) {
  const [theme, setTheme] = useState<"light" | "dark">(
    () => (localStorage.getItem("theme") as "light" | "dark") ??
      (matchMedia("(prefers-color-scheme: dark)").matches ? "dark" : "light")
  );

  useEffect(() => {
    document.documentElement.dataset.theme = theme;
    localStorage.setItem("theme", theme);
  }, [theme]);

  return (
    <ThemeContext.Provider value={{ theme, setTheme }}>
      {children}
    </ThemeContext.Provider>
  );
}
```

**Real World Example:** `ROLES` gives one typed source for role strings; a typo becomes a compile error.

**Production Example:** The theme provider respects system preference on first load and persists the user's choice.

**Explanation:** Centralizing config, constants, and theming removes scattered magic values and keeps the app cohesive.

## Common Mistakes

- Hard-coding URLs/keys instead of env variables.
- Magic strings/numbers scattered across features.
- Utility grab-bag files with unrelated helpers.
- Ad-hoc theming instead of a centralized provider.

## AI Instructions

- Read config from typed env; never hard-code URLs/keys.
- Extract magic values into named, single-sourced constants.
- Keep helpers pure and single-purpose; share only on real reuse.
- Centralize theming in the provider using tokens and system preference.

## Checklist

- [ ] Is all config from typed env variables?
- [ ] Are magic values extracted into constants?
- [ ] Are helpers pure, typed, single-purpose?
- [ ] Is theming centralized and system-aware?

## Summary

The best practices glue the app together: typed env config, single-sourced constants, pure helpers, disciplined reuse, and centralized theming with dark/light support. These cross-cutting habits keep the whole codebase consistent, secure, and maintainable.

## 🔗 Related Chapters

Read next:

- 05-dry-kiss-yagni.md
- 11-vite.md
- 17-authentication.md
