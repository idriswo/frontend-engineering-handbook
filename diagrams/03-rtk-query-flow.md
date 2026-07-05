# RTK Query Flow

Caching, hooks, and tag-based invalidation. See [14-rtk-query.md](../docs/14-rtk-query.md).

```mermaid
graph LR
  COMP[Component] -->|useGetUsersQuery| CACHE{Cache hit?}
  CACHE -->|yes| RETURN[Return cached data]
  CACHE -->|no| BASEQ[baseQuery / Axios]
  BASEQ --> BE[(Backend)]
  BE --> STORE[RTK Query cache]
  STORE --> PROVIDE["providesTags: [Users]"]
  STORE --> RETURN

  COMP2[Component] -->|useCreateUserMutation| MUT[Mutation]
  MUT --> BE
  MUT --> INVAL["invalidatesTags: [Users]"]
  INVAL -.refetch.-> STORE
```

**Key idea:** queries provide tags, mutations invalidate them, and cached reads refresh automatically — no manual wiring.
