# React Router

## Purpose

This chapter defines how we handle routing with **React Router DOM**: route configuration, nested layouts, lazy loading, protected routes, and navigation.

## Why It Matters

Routing is the skeleton of a single-page app. A clean routing setup enables code splitting, consistent layouts, and access control. A messy one produces duplicated layout logic, unprotected routes, and large initial bundles.

## Core Concepts

**Centralized route config.** Routes are defined in one place (`app/router.tsx`) using the data router API.

**Nested layouts.** Shared chrome (nav, sidebar) is a layout route wrapping children via `<Outlet />`.

**Lazy routes.** Page components are `React.lazy`-loaded so each route is its own chunk.

**Route guards.** Protected routes wrap children in an auth check (see chapter 17).

## Rules

1. Define routes centrally with `createBrowserRouter`.
2. Use layout routes with `<Outlet />` for shared chrome.
3. Lazy-load page components; wrap in `<Suspense>`.
4. Protect private routes with a guard component.
5. Use typed route paths/constants — no magic path strings scattered around.
6. Handle 404 with a catch-all route.

## Best Practices

- Keep a `ROUTES` constant map as the single source of path truth.
- Co-locate route elements with their features.
- Use `useNavigate`/`<Link>`; never mutate `window.location` for in-app navigation.

## Bad Examples

```tsx
// ❌ Inline everything, magic strings, no lazy loading, no guard.
<BrowserRouter>
  <Routes>
    <Route path="/dashboard" element={<Dashboard />} />
    <Route path="/admin" element={<Admin />} />
  </Routes>
</BrowserRouter>
```

## Good Examples

```tsx
// app/router.tsx
import { createBrowserRouter } from "react-router-dom";
import { lazy } from "react";
import { AppLayout } from "@/app/AppLayout";
import { ProtectedRoute } from "@/features/auth";

const Dashboard = lazy(() => import("@/features/dashboard"));

export const ROUTES = { dashboard: "/dashboard" } as const;

export const router = createBrowserRouter([
  {
    element: <AppLayout />,
    children: [
      {
        path: ROUTES.dashboard,
        element: (
          <ProtectedRoute>
            <Dashboard />
          </ProtectedRoute>
        ),
      },
      { path: "*", element: <NotFound /> },
    ],
  },
]);
```

**Real World Example:** Every private page reuses `<ProtectedRoute>` and `<AppLayout>` — no duplication.

**Production Example:** `lazy` splits the dashboard into its own chunk, shrinking the initial bundle.

**Explanation:** Central config + layouts + guards + lazy loading give a scalable, secure, performant router.

## Common Mistakes

- Magic path strings duplicated across the app.
- Forgetting `<Suspense>` around lazy routes.
- Unprotected private routes.
- Layout logic copy-pasted into each page.

## AI Instructions

- Add routes to the central router config.
- Wrap private routes in the guard component.
- Lazy-load page components with `<Suspense>` fallback.
- Reference paths via the `ROUTES` constant.

## Checklist

- [ ] Routes defined centrally?
- [ ] Shared chrome via layout routes and `<Outlet />`?
- [ ] Page components lazy-loaded with Suspense?
- [ ] Private routes guarded?
- [ ] Paths sourced from a constants map?

## Summary

Routing is centralized, layout-driven, lazy-loaded, and guarded. A single `ROUTES` map is the source of path truth, layouts eliminate chrome duplication, and lazy routes keep the initial bundle small.

## 🔗 Related Chapters

Read next:

- 06-project-architecture.md
- 17-authentication.md
- 24-performance.md
