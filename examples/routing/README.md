# Routing Example

Nested routes, layouts, and lazy loading. Maps to [12-react-router.md](../../docs/12-react-router.md) and [diagrams/05-routing.md](../../diagrams/05-routing.md).

## Scenario

A public/auth split with a protected dashboard and lazy pages.

## Code

```tsx
// src/app/router.tsx
import { lazy, Suspense } from "react";
import { createBrowserRouter } from "react-router-dom";
import { AppLayout } from "@/components/layout/AppLayout";
import { ProtectedRoute } from "@/features/auth/components/ProtectedRoute";
import { Spinner } from "@/components/ui/Spinner";

const DashboardPage = lazy(() => import("@/pages/DashboardPage"));
const UsersPage = lazy(() => import("@/pages/UsersPage"));
const LoginPage = lazy(() => import("@/pages/LoginPage"));

const withSuspense = (el: React.ReactNode) => <Suspense fallback={<Spinner />}>{el}</Suspense>;

export const router = createBrowserRouter([
  { path: "/login", element: withSuspense(<LoginPage />) },
  {
    element: <ProtectedRoute />,
    children: [
      {
        element: <AppLayout />,
        children: [
          { path: "/", element: withSuspense(<DashboardPage />) },
          { path: "/users", element: withSuspense(<UsersPage />) },
        ],
      },
    ],
  },
  { path: "*", element: <div>404 — Not found</div> },
]);
```

## Explanation

- `React.lazy` + `Suspense` code-split each page — smaller initial bundle.
- Guard and layout are route wrappers via `<Outlet />` — no repeated boilerplate per page.
- A catch-all `*` route handles 404s.

## Related

[12-react-router.md](../../docs/12-react-router.md) · [layout template](../../templates/layout-template.md)
