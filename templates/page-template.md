# Page Template

A route-level page component: composes feature components, handles data states.
See [12-react-router.md](../docs/12-react-router.md) and [25-error-handling.md](../docs/25-error-handling.md).

## Template

```tsx
// UsersPage.tsx
import { useGetUsersQuery } from "@/features/users/api/usersApi";
import { UserList } from "@/features/users/components/UserList";
import { PageHeader } from "@/components/layout/PageHeader";
import { Spinner } from "@/components/ui/Spinner";
import { ErrorState } from "@/components/ui/ErrorState";

export default function UsersPage() {
  const { data: users, isLoading, isError, refetch } = useGetUsersQuery();

  return (
    <section className="space-y-6">
      <PageHeader title="Users" description="Manage application users" />

      {isLoading && <Spinner />}
      {isError && <ErrorState onRetry={refetch} />}
      {users && <UserList users={users} />}
    </section>
  );
}
```

## Rules

- Pages are **thin**: they fetch/select data and compose feature components — no complex logic.
- Always handle the three states explicitly: **loading**, **error**, **success**.
- `export default` for lazy-loaded routes (`React.lazy` + `Suspense`).
- Keep page-specific layout here; shared layout belongs in a layout component.
- Set document title / metadata if the app requires it.

## Checklist

- [ ] Loading, error, and empty states handled
- [ ] Default export for lazy routes
- [ ] No business logic — delegated to features/hooks
- [ ] Data fetched via RTK Query / selectors, not ad-hoc `fetch`
- [ ] Accessible landmarks (`section`, headings)
