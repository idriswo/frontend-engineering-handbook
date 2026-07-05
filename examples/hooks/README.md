# Hooks Example

A custom hook that encapsulates reusable logic. Maps to [09-react.md](../../docs/09-react.md) and the [hook template](../../templates/hook-template.md).

## Scenario

`useLocalStorage` — state that persists to `localStorage`.

## Code

```ts
// src/hooks/useLocalStorage.ts
import { useCallback, useEffect, useState } from "react";

export function useLocalStorage<T>(key: string, initial: T) {
  const [value, setValue] = useState<T>(() => {
    try {
      const stored = localStorage.getItem(key);
      return stored ? (JSON.parse(stored) as T) : initial;
    } catch {
      return initial;
    }
  });

  useEffect(() => {
    localStorage.setItem(key, JSON.stringify(value));
  }, [key, value]);

  const reset = useCallback(() => setValue(initial), [initial]);

  return { value, setValue, reset } as const;
}
```

```tsx
// Usage
const { value: theme, setValue: setTheme } = useLocalStorage<"light" | "dark">("theme", "light");
```

## Explanation

- Generic `<T>` keeps the hook type-safe for any stored value.
- Lazy `useState` initializer reads storage once, not on every render.
- Reads are guarded with `try/catch` (storage can throw or hold bad JSON).
- Returns a stable, typed object (`as const`).

## Related

[Hook template](../../templates/hook-template.md) · [react-hooks-cheatsheet.md](../../reference/react-hooks-cheatsheet.md)
