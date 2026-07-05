# React Rendering

How React renders and re-renders. See [09-react.md](../docs/09-react.md) and [24-performance.md](../docs/24-performance.md).

```mermaid
graph TD
  TRIGGER[State / props change] --> RENDER[Render phase]
  RENDER --> VDOM[Compute new virtual DOM]
  VDOM --> DIFF[Reconciliation / diff]
  DIFF --> COMMIT[Commit phase]
  COMMIT --> DOM[Update real DOM]
  COMMIT --> EFFECTS[Run effects useEffect]

  subgraph Optimization
    MEMO[React.memo] -.skips.-> RENDER
    USEMEMO[useMemo] -.caches value.-> RENDER
    USECB[useCallback] -.stable fn.-> MEMO
  end
```

**Key idea:** re-render is triggered by state/prop changes; memoization skips work only when props are referentially stable — and only where profiling justifies it.
