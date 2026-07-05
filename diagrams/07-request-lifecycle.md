# Request Lifecycle

Full lifecycle of a data request through the stack. See [16-axios.md](../docs/16-axios.md) and [25-error-handling.md](../docs/25-error-handling.md).

```mermaid
sequenceDiagram
  participant C as Component
  participant RQ as RTK Query hook
  participant AX as Axios instance
  participant REQ as Request interceptor
  participant BE as Backend
  participant RES as Response interceptor

  C->>RQ: useGetXQuery()
  RQ-->>C: isLoading = true (show skeleton)
  RQ->>AX: baseQuery
  AX->>REQ: attach auth token
  REQ->>BE: HTTP request
  alt success
    BE-->>RES: 200 + data
    RES-->>RQ: normalized data
    RQ-->>C: isSuccess (render) or empty state
  else error
    BE-->>RES: 4xx/5xx
    RES->>RES: normalizeError / handle 401
    RES-->>RQ: rejected
    RQ-->>C: isError (show error + retry)
  end
```

**Key idea:** every request passes through interceptors for auth and error normalization; the UI always reflects loading, success, empty, or error.
