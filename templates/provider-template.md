# Provider Template

A typed React Context provider for cross-cutting state (theme, auth, i18n).
See [30-best-practices.md](../docs/30-best-practices.md).

## Template

```tsx
// ThemeProvider.tsx
import { createContext, useContext, useMemo, useState, type ReactNode } from "react";

type Theme = "light" | "dark";

interface ThemeContextValue {
  theme: Theme;
  toggle: () => void;
}

const ThemeContext = createContext<ThemeContextValue | null>(null);

export function ThemeProvider({ children }: { children: ReactNode }) {
  const [theme, setTheme] = useState<Theme>("light");

  const value = useMemo<ThemeContextValue>(
    () => ({ theme, toggle: () => setTheme((t) => (t === "light" ? "dark" : "light")) }),
    [theme],
  );

  return <ThemeContext.Provider value={value}>{children}</ThemeContext.Provider>;
}

export function useTheme(): ThemeContextValue {
  const ctx = useContext(ThemeContext);
  if (!ctx) throw new Error("useTheme must be used within <ThemeProvider>");
  return ctx;
}
```

## Rules

- Ship a custom hook (`useTheme`) that throws if used outside the provider — no `null` leaks.
- Memoize the context value so consumers don't re-render on every parent render.
- Keep context **small and focused**; split unrelated concerns into separate providers.
- Don't use context for high-frequency state (use Redux/local state instead).

## Checklist

- [ ] Context value typed, no implicit `any`
- [ ] Value memoized with `useMemo`
- [ ] Guarded consumer hook that throws outside provider
- [ ] Single responsibility per provider
