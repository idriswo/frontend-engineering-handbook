# Authentication Flow

JWT access/refresh token flow with route guards. See [17-authentication.md](../docs/17-authentication.md).

```mermaid
sequenceDiagram
  participant U as User
  participant App as React App
  participant Guard as ProtectedRoute
  participant API as Axios + Interceptor
  participant BE as Backend

  U->>App: Open app
  App->>API: restoreSession (refresh token)
  API->>BE: POST /auth/refresh
  BE-->>API: new access token + user
  API-->>App: session hydrated

  U->>Guard: Navigate to /dashboard
  alt authenticated & authorized
    Guard-->>U: Render page
  else not authenticated
    Guard-->>U: Redirect to /login
  else wrong role
    Guard-->>U: Redirect to /forbidden
  end

  U->>API: Request with expired token
  API->>BE: 401 Unauthorized
  API->>BE: POST /auth/refresh
  alt refresh ok
    BE-->>API: new access token
    API->>BE: retry original request
  else refresh fails
    API-->>App: dispatch(logout)
  end
```

**Key idea:** tokens are handled centrally by interceptors; guards enforce access; the server is the security authority.
