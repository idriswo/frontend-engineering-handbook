# Feature Template

A self-contained, feature-based module. Everything a feature needs lives in one folder.
See [06-project-architecture.md](../docs/06-project-architecture.md) and [07-folder-structure.md](../docs/07-folder-structure.md).

## Structure

```
features/users/
├── api/
│   └── usersApi.ts        # RTK Query endpoints
├── components/
│   ├── UserList.tsx
│   └── UserCard.tsx
├── hooks/
│   └── useUserFilters.ts
├── types/
│   └── user.types.ts
├── utils/
│   └── formatUser.ts
└── index.ts               # Public surface of the feature
```

## Public surface (`index.ts`)

```ts
// Only export what the rest of the app is allowed to use.
export { UserList } from "./components/UserList";
export { useGetUsersQuery } from "./api/usersApi";
export type { User } from "./types/user.types";
```

## Rules

- A feature owns its components, hooks, API, types, and utils.
- **Import across features only through the feature's `index.ts`** — never reach into internals.
- No feature imports another feature's internal files (prevents tangled coupling).
- Shared, cross-feature code lives in `src/components`, `src/lib`, or `src/hooks`.
- Keep features deletable: removing the folder should not break unrelated code.

## Checklist

- [ ] Folder contains only this feature's code
- [ ] Public API exposed via `index.ts`
- [ ] No deep imports into other features
- [ ] Types colocated in `types/`
- [ ] Feature is independently testable
