# createAsyncThunk

## Purpose

This chapter defines when and how to use **`createAsyncThunk`** for async logic that belongs in Redux — and clarifies why it is the *exception*, not the default, in a project that uses RTK Query.

## Why It Matters

RTK Query handles almost all server interactions. But some async flows are genuinely *client-state* transitions — complex multi-step logic, non-HTTP async work, or coordinating several slices. For those, `createAsyncThunk` provides a standard lifecycle (`pending`/`fulfilled`/`rejected`). Knowing when to reach for it (rarely) prevents duplicating RTK Query's job.

## Core Concepts

**Default to RTK Query.** For data fetching/caching, use RTK Query. Reach for a thunk only when the async work is about client state, not cached server data.

**Lifecycle actions.** A thunk auto-generates `pending`, `fulfilled`, `rejected` handled in `extraReducers`.

**`rejectWithValue`.** Return typed error payloads instead of throwing raw errors.

## Rules

1. Prefer RTK Query for server data; use `createAsyncThunk` only for client-state async flows.
2. Type the argument, return value, and rejection value.
3. Handle all three lifecycle states in `extraReducers`.
4. Use `rejectWithValue` for structured error handling.
5. Keep thunks thin — delegate real work to services.

## Best Practices

- Co-locate the thunk with its slice.
- Return normalized, serializable payloads.
- Cancel or guard against race conditions where relevant.

## Bad Examples

```tsx
// ❌ Using a thunk to fetch cacheable server data (RTK Query's job) and untyped errors.
export const fetchUsers = createAsyncThunk("users/fetch", async () => {
  const res = await axios.get("/users");
  return res.data; // no types, no cache, duplicates RTK Query
});
```

## Good Examples

```tsx
// A legitimate client-state async flow: hydrate session from storage + validate.
export const restoreSession = createAsyncThunk<
  { user: User; token: string },
  void,
  { rejectValue: string }
>("auth/restoreSession", async (_, { rejectWithValue }) => {
  const token = localStorage.getItem("token");
  if (!token) return rejectWithValue("No session");
  try {
    const user = await authService.validate(token);
    return { user, token };
  } catch {
    return rejectWithValue("Invalid session");
  }
});

// slice extraReducers
extraReducers: (builder) => {
  builder
    .addCase(restoreSession.pending, (state) => {
      state.status = "loading";
    })
    .addCase(restoreSession.fulfilled, (state, action) => {
      state.status = "authenticated";
      state.user = action.payload.user;
      state.token = action.payload.token;
    })
    .addCase(restoreSession.rejected, (state) => {
      state.status = "unauthenticated";
    });
};
```

**Real World Example:** Session restoration touches storage + validation + auth slice — client-state coordination, a fitting thunk use.

**Production Example:** Typed `rejectWithValue` gives the UI a clean error message without leaking raw exceptions.

**Explanation:** The thunk manages *client* state transitions; RTK Query still owns all cached server reads.

## Common Mistakes

- Using thunks to fetch cacheable server data.
- Not handling `rejected`, leaving stuck loading states.
- Untyped arguments and errors.
- Fat thunks containing business logic that belongs in a service.

## AI Instructions

- Default to RTK Query; only use `createAsyncThunk` for client-state async flows.
- Type argument, return, and `rejectValue`.
- Handle pending/fulfilled/rejected in `extraReducers`.
- Keep the thunk thin; delegate to a service.

## Checklist

- [ ] Is this genuinely client-state async (not RTK Query's job)?
- [ ] Are arg/return/reject types defined?
- [ ] Are all three lifecycle states handled?
- [ ] Is error handling done via `rejectWithValue`?

## Summary

`createAsyncThunk` is the exception, not the rule. Use it only for client-state async flows that RTK Query does not cover, always typed and with full lifecycle handling. For server data, RTK Query remains the default.

## 🔗 Related Chapters

Read next:

- 13-redux-toolkit.md
- 14-rtk-query.md
- 16-axios.md
