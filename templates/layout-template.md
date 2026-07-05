# Layout Template

A shared layout wrapping routes with persistent chrome (header, sidebar, footer).
See [12-react-router.md](../docs/12-react-router.md).

## Template

```tsx
// AppLayout.tsx
import { Outlet } from "react-router-dom";
import { Header } from "@/components/layout/Header";
import { Sidebar } from "@/components/layout/Sidebar";

export function AppLayout() {
  return (
    <div className="flex min-h-screen flex-col">
      <Header />
      <div className="flex flex-1">
        <Sidebar />
        <main className="flex-1 p-6">
          {/* Child routes render here */}
          <Outlet />
        </main>
      </div>
    </div>
  );
}
```

## Router wiring

```tsx
{
  element: <AppLayout />,
  children: [
    { path: "/", element: <DashboardPage /> },
    { path: "/users", element: <UsersPage /> },
  ],
}
```

## Rules

- Use `<Outlet />` for nested route content — never hardcode children.
- Layouts hold persistent UI (nav, theme, containers), not page logic.
- Compose layouts (public / auth / dashboard) instead of one mega-layout.
- Keep layout state (sidebar open, theme) minimal and, when shared, in a provider or store.

## Checklist

- [ ] Renders `<Outlet />`
- [ ] No page-specific logic
- [ ] Responsive across breakpoints
- [ ] Uses semantic landmarks (`header`, `nav`, `main`, `footer`)
