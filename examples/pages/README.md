# Pages Example

A thin route-level page handling data states. Maps to the [page template](../../templates/page-template.md).

## Scenario

A users page that fetches data and renders loading/error/success states.

## Code

```tsx
// src/pages/UsersPage.tsx
import { useGetUsersQuery } from "@/features/users/api/usersApi";
import { UserList } from "@/features/users/components/UserList";
import { Spinner } from "@/components/ui/Spinner";
import { ErrorState } from "@/components/ui/ErrorState";
import { EmptyState } from "@/components/ui/EmptyState";

export default function UsersPage() {
  const { data: users, isLoading, isError, refetch } = useGetUsersQuery();

  if (isLoading) return <Spinner />;
  if (isError) return <ErrorState onRetry={refetch} />;
  if (!users?.length) return <EmptyState message="No users yet" />;

  return (
    <section className="space-y-6">
      <h1 className="text-2xl font-semibold">Users</h1>
      <UserList users={users} />
    </section>
  );
}
```

## Explanation

- The page is **thin**: it fetches data and composes feature components — no business logic.
- All four states handled: loading, error, empty, success.
- `export default` enables lazy loading in the router.

## Related

[Page template](../../templates/page-template.md) · [25-error-handling.md](../../docs/25-error-handling.md)
