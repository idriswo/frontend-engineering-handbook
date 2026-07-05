# Redux Toolkit

## Purpose

This chapter defines how we use **Redux Toolkit (RTK)** for global *client* state: store setup, slices, typed hooks, and when Redux is (and is not) the right tool.

## Why It Matters

Global state is easy to overuse. RTK gives a standardized, boilerplate-free, immutable-by-default way to manage the state that genuinely needs to be global (auth, theme, UI shell). Used wisely it centralizes cross-cutting state; used carelessly it becomes a dumping ground that couples the whole app.

## Core Concepts

**Server state vs. client state.** Server data belongs to RTK Query (chapter 14). Redux slices hold *client* state only: auth session, theme, modals, feature flags.

**Slices.** A slice bundles state + reducers + actions with Immer-powered "mutating" syntax.

**Typed hooks.** Use `useAppSelector`/`useAppDispatch` typed to the store — never the raw hooks.

**Single store.** One store composed in `app/store.ts`.

## Rules

1. Use Redux only for global client state, not server data.
2. Define state with `createSlice`; never hand-write action types.
3. Export and use typed `useAppSelector`/`useAppDispatch`.
4. Keep slices small and feature-scoped.
5. Never store derivable data — compute with selectors.
6. Do not put non-serializable values in the store.

## Best Practices

- Co-locate a slice with its feature (`features/auth/authSlice.ts`).
- Use memoized selectors for derived data.
- Keep reducers pure; side effects go in thunks or RTK Query.

## Bad Examples

```tsx
// ❌ Server data in Redux, raw hooks, manual action types.
const usersReducer = (state = [], action) => {
  if (action.type === "SET_USERS") return action.payload;
  return state;
};
const users = useSelector((s: any) => s.users);
```

## Good Examples

```tsx
// features/auth/authSlice.ts
import { createSlice, type PayloadAction } from "@reduxjs/toolkit";

interface AuthState {
  user: User | null;
  token: string | null;
}

const initialState: AuthState = { user: null, token: null };

const authSlice = createSlice({
  name: "auth",
  initialState,
  reducers: {
    setCredentials(state, action: PayloadAction<{ user: User; token: string }>) {
      state.user = action.payload.user;
      state.token = action.payload.token;
    },
    logout(state) {
      state.user = null;
      state.token = null;
    },
  },
});

export const { setCredentials, logout } = authSlice.actions;
export default authSlice.reducer;
```

```ts
// app/hooks.ts — typed hooks
export const useAppDispatch = () => useDispatch<AppDispatch>();
export const useAppSelector: TypedUseSelectorHook<RootState> = useSelector;
```

**Real World Example:** Auth state lives in one slice; any component reads `user` via the typed selector.

**Production Example:** Immer lets `setCredentials` "mutate" safely while keeping state immutable under the hood.

**Explanation:** RTK removes boilerplate and enforces immutability; typed hooks give end-to-end safety.

## Common Mistakes

- Storing server data in Redux instead of RTK Query.
- Using raw `useSelector`/`useDispatch` (untyped).
- Storing derivable/computed values.
- One giant slice for everything.

## AI Instructions

- Use Redux only for global client state.
- Create state with `createSlice` and typed payloads.
- Use `useAppSelector`/`useAppDispatch`.
- Keep slices feature-scoped and small.

## Checklist

- [ ] Is this genuinely global *client* state (not server data)?
- [ ] Defined with `createSlice`?
- [ ] Using typed hooks?
- [ ] No derivable data stored?
- [ ] Slice small and feature-scoped?

## Summary

Redux Toolkit manages global *client* state only. Slices with typed payloads, typed hooks, and memoized selectors keep global state safe and minimal — while server state stays in RTK Query.
