# Custom Hook Template

Encapsulate reusable stateful logic in a typed custom hook.
See [09-react.md](../docs/09-react.md).

## Template

```ts
// useDebouncedValue.ts
import { useEffect, useState } from "react";

/**
 * Returns a debounced copy of `value` that only updates
 * after `delay` ms have passed without a change.
 */
export function useDebouncedValue<T>(value: T, delay = 300): T {
  const [debounced, setDebounced] = useState<T>(value);

  useEffect(() => {
    const id = setTimeout(() => setDebounced(value), delay);
    return () => clearTimeout(id);
  }, [value, delay]);

  return debounced;
}
```

## Usage

```tsx
const [query, setQuery] = useState("");
const debouncedQuery = useDebouncedValue(query, 400);

const { data } = useSearchQuery(debouncedQuery, { skip: !debouncedQuery });
```

## Rules

- Prefix with `use`; call hooks unconditionally at the top level.
- Return a typed value, tuple, or object — keep the shape stable.
- Clean up subscriptions, timers, and listeners in the effect's return function.
- Keep one responsibility per hook; compose small hooks instead of one giant one.
- No JSX inside hooks.

## Checklist

- [ ] Name starts with `use`
- [ ] Generic/typed inputs and return
- [ ] Effects cleaned up
- [ ] Dependency array correct and exhaustive
- [ ] No conditional hook calls
