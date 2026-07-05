# Routing

Nested layouts, lazy routes, and guards. See [12-react-router.md](../docs/12-react-router.md).

```mermaid
graph TD
  ROUTER[createBrowserRouter] --> LAYOUT[AppLayout + Outlet]
  LAYOUT --> PUBLIC[Public routes]
  LAYOUT --> PRIVATE[ProtectedRoute]
  LAYOUT --> NOTFOUND["* NotFound"]

  PUBLIC --> LOGIN[/login/]
  PRIVATE --> DASH[lazy Dashboard]
  PRIVATE --> ADMIN[lazy Admin requiredRole=admin]

  DASH --> SUSPENSE[Suspense fallback = Skeleton]
  ADMIN --> SUSPENSE
```

**Key idea:** one central config, shared layout via `<Outlet />`, lazy-loaded pages, and guarded private routes.
