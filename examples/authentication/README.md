# Authentication Example

Protected routes with JWT + a route guard. Maps to [17-authentication.md](../../docs/17-authentication.md) and [diagrams/02-authentication-flow.md](../../diagrams/02-authentication-flow.md).

## Scenario

Guard private routes; redirect unauthenticated users to `/login`.

## Code

```tsx
// src/features/auth/components/ProtectedRoute.tsx
import { Navigate, Outlet, useLocation } from "react-router-dom";
import { useAppSelector } from "@/app/hooks";

export function ProtectedRoute({ requiredRole }: { requiredRole?: string }) {
  const { user, isAuthenticated } = useAppSelector((s) => s.auth);
  const location = useLocation();

  if (!isAuthenticated) {
    return <Navigate to="/login" replace state={{ from: location }} />;
  }

  if (requiredRole && user?.role !== requiredRole) {
    return <Navigate to="/forbidden" replace />;
  }

  return <Outlet />;
}
```

```tsx
// Router wiring
{
  element: <ProtectedRoute requiredRole="admin" />,
  children: [{ path: "/admin", element: <AdminPage /> }],
}
```

## Explanation

- The guard renders `<Outlet />` only when authenticated **and** authorized.
- `state={{ from: location }}` lets `/login` redirect back after sign-in.
- Authorization here is **UX only** — the server must re-check on every request.

## Related

[17-authentication.md](../../docs/17-authentication.md) · [checklists/security.md](../../checklists/security.md)
