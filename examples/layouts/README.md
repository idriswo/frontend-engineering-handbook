# Layouts Example

Composable layouts with a shared shell. Maps to the [layout template](../../templates/layout-template.md).

## Scenario

An authenticated app shell (header + sidebar) and a minimal auth layout, both reused across routes.

## Code

```tsx
// src/components/layout/AppLayout.tsx
import { Outlet } from "react-router-dom";
import { Header } from "./Header";
import { Sidebar } from "./Sidebar";

export function AppLayout() {
  return (
    <div className="flex min-h-screen flex-col">
      <Header />
      <div className="flex flex-1">
        <Sidebar />
        <main className="flex-1 p-6">
          <Outlet />
        </main>
      </div>
    </div>
  );
}
```

```tsx
// src/components/layout/AuthLayout.tsx — minimal, centered
import { Outlet } from "react-router-dom";

export function AuthLayout() {
  return (
    <main className="grid min-h-screen place-items-center bg-muted p-4">
      <div className="w-full max-w-sm rounded-lg bg-background p-6 shadow">
        <Outlet />
      </div>
    </main>
  );
}
```

## Explanation

- Each layout renders `<Outlet />` for its child routes — pages stay layout-agnostic.
- Splitting `AppLayout` and `AuthLayout` avoids conditional chrome inside pages.
- Semantic landmarks (`header`, `main`) improve accessibility.

## Related

[Layout template](../../templates/layout-template.md) · [routing example](../routing/README.md)
