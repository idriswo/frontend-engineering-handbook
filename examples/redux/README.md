# Redux Toolkit Example

A typed slice for client state. Maps to [13-redux-toolkit.md](../../docs/13-redux-toolkit.md).

## Scenario

An `auth` slice holding the current user and session flags.

## Code

```ts
// src/features/auth/authSlice.ts
import { createSlice, type PayloadAction } from "@reduxjs/toolkit";

interface User {
  id: string;
  name: string;
  role: string;
}

interface AuthState {
  user: User | null;
  isAuthenticated: boolean;
}

const initialState: AuthState = { user: null, isAuthenticated: false };

const authSlice = createSlice({
  name: "auth",
  initialState,
  reducers: {
    setUser(state, action: PayloadAction<User>) {
      state.user = action.payload;
      state.isAuthenticated = true;
    },
    logout(state) {
      state.user = null;
      state.isAuthenticated = false;
    },
  },
});

export const { setUser, logout } = authSlice.actions;
export default authSlice.reducer;
```

```ts
// src/app/hooks.ts — typed hooks
import { useDispatch, useSelector } from "react-redux";
import type { RootState, AppDispatch } from "./store";

export const useAppDispatch = () => useDispatch<AppDispatch>();
export const useAppSelector = useSelector.withTypes<RootState>();
```

## Explanation

- Immer (built into RTK) lets you "mutate" state safely inside reducers.
- `PayloadAction<T>` types the action payload.
- Typed `useAppSelector`/`useAppDispatch` avoid repeating generics everywhere.
- **Client state only** — server data belongs in RTK Query, not a slice.

## Related

[13-redux-toolkit.md](../../docs/13-redux-toolkit.md) · [rtk-query example](../rtk-query/README.md)
