# RTK Query

## Purpose

This chapter defines how we manage **server state** with **RTK Query**: API slices, endpoints, caching, invalidation, and generated hooks. RTK Query is the default data-fetching layer of the app.

## Why It Matters

Server state is fundamentally different from client state: it is cached, shared, and can go stale. RTK Query handles caching, deduplication, background refetching, loading/error states, and cache invalidation out of the box. Rolling this by hand with `useEffect` + `useState` reproduces bugs that RTK Query already solved.

## Core Concepts

**API slice.** A single `createApi` per base URL defines endpoints and generates hooks.

**Queries vs. mutations.** Queries read; mutations write and invalidate cached tags.

**Tag-based invalidation.** Endpoints `provide` and `invalidate` tags so writes refresh the right reads automatically.

**Generated hooks.** `useGetUsersQuery`, `useCreateUserMutation` — typed and cache-aware.

## Rules

1. All server data goes through RTK Query, never raw `fetch`/`axios` in components.
2. One API slice per base URL; endpoints injected per feature.
3. Use tags to invalidate related caches after mutations.
4. Always handle `isLoading`, `isError`, and empty data in the UI.
5. Type endpoint request and response payloads.
6. Configure the base query with the shared Axios/auth setup.

## Best Practices

- Inject endpoints per feature with `injectEndpoints` to keep features modular.
- Use `providesTags`/`invalidatesTags` precisely to avoid over-fetching.
- Prefer optimistic updates for snappy UX where safe.

## Bad Examples

```tsx
// ❌ Manual fetching reinvents caching, states, and invalidation.
const [users, setUsers] = useState<User[]>([]);
useEffect(() => {
  axios.get("/users").then((r) => setUsers(r.data));
}, []);
```

## Good Examples

```tsx
// features/users/usersApi.ts
import { baseApi } from "@/app/baseApi";

export const usersApi = baseApi.injectEndpoints({
  endpoints: (build) => ({
    getUsers: build.query<User[], void>({
      query: () => "/users",
      providesTags: ["Users"],
    }),
    createUser: build.mutation<User, CreateUserDto>({
      query: (body) => ({ url: "/users", method: "POST", body }),
      invalidatesTags: ["Users"],
    }),
  }),
});

export const { useGetUsersQuery, useCreateUserMutation } = usersApi;
```

```tsx
// ✅ UI handles every state; cache refreshes automatically after create.
function Users() {
  const { data: users, isLoading, isError } = useGetUsersQuery();
  const [createUser] = useCreateUserMutation();

  if (isLoading) return <UsersSkeleton />;
  if (isError) return <ErrorState message="Failed to load users." />;
  if (!users?.length) return <EmptyState message="No users yet." />;

  return <UserTable users={users} onCreate={createUser} />;
}
```

**Real World Example:** After `createUser`, the `"Users"` tag invalidates and the list refetches with no manual wiring.

**Production Example:** Two components using `useGetUsersQuery` share one cached request — automatic deduplication.

**Explanation:** RTK Query turns caching, states, and invalidation into declarative configuration.

## Common Mistakes

- Fetching server data with `useEffect` instead of RTK Query.
- Forgetting `invalidatesTags`, leaving stale UI after writes.
- Not handling loading/error/empty states.
- Multiple API slices for the same base URL.

## AI Instructions

- Route all server data through RTK Query.
- Define typed queries/mutations with precise tags.
- Handle loading, error, and empty states in the UI.
- Inject endpoints per feature into the shared base API.

## Checklist

- [ ] Is all server data fetched via RTK Query?
- [ ] Are request/response types defined?
- [ ] Are tags provided/invalidated correctly?
- [ ] Are loading, error, empty states handled?

## Summary

RTK Query is the server-state layer: declarative endpoints, automatic caching and deduplication, and tag-based invalidation. Components consume generated typed hooks and always handle loading, error, and empty states.
