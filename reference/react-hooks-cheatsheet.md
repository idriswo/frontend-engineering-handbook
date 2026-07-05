# React Hooks Cheat Sheet

Quick reference for React 19 hooks. See [09-react.md](../docs/09-react.md) and [24-performance.md](../docs/24-performance.md).

## State

```tsx
const [count, setCount] = useState(0);
setCount((prev) => prev + 1);        // functional update (safe for batching)

const [state, dispatch] = useReducer(reducer, initialState);
dispatch({ type: "increment" });
```

## Effects

```tsx
useEffect(() => {
  const id = subscribe();
  return () => unsubscribe(id);      // cleanup
}, [dep]);                            // runs when dep changes

useLayoutEffect(() => { /* sync DOM read/write before paint */ }, []);
```

| Deps | Runs |
| --- | --- |
| `[]` | Once on mount |
| `[a, b]` | On mount + when a or b changes |
| omitted | Every render (avoid) |

## Refs

```tsx
const inputRef = useRef<HTMLInputElement>(null);
inputRef.current?.focus();

const countRef = useRef(0);           // mutable value, no re-render
```

## Performance

```tsx
const value = useMemo(() => expensive(a, b), [a, b]);   // cache value
const fn = useCallback(() => doThing(id), [id]);        // stable function
const Memoized = React.memo(Component);                 // skip re-render on equal props
```

## Context

```tsx
const ThemeContext = createContext<Theme | null>(null);
const theme = useContext(ThemeContext);
```

## React 19 additions

```tsx
const value = use(promiseOrContext);        // read promise/context (Suspense-aware)
const [state, action, pending] = useActionState(fn, initial);
const { pending } = useFormStatus();
const optimistic = useOptimistic(state, updateFn);
useTransition();                             // const [isPending, startTransition] = useTransition()
const id = useId();                          // stable unique id for a11y
```

## Rules of Hooks

- Call hooks **only at the top level** — never in loops, conditions, or nested functions.
- Call hooks **only from React functions** (components or custom hooks).
- Custom hooks start with `use`.
