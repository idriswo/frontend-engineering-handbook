# API Slice Template

An RTK Query API slice: typed endpoints, caching, and tag-based invalidation.
See [14-rtk-query.md](../docs/14-rtk-query.md) and [13-redux-toolkit.md](../docs/13-redux-toolkit.md).

## Base API

```ts
// app/api/baseApi.ts
import { createApi, fetchBaseQuery } from "@reduxjs/toolkit/query/react";

export const baseApi = createApi({
  reducerPath: "api",
  baseQuery: fetchBaseQuery({
    baseUrl: import.meta.env.VITE_API_URL,
    prepareHeaders: (headers) => {
      const token = localStorage.getItem("accessToken");
      if (token) headers.set("Authorization", `Bearer ${token}`);
      return headers;
    },
  }),
  tagTypes: ["User"],
  endpoints: () => ({}),
});
```

## Feature endpoints (injected)

```ts
// features/users/api/usersApi.ts
import { baseApi } from "@/app/api/baseApi";
import type { User } from "../types/user.types";

export const usersApi = baseApi.injectEndpoints({
  endpoints: (build) => ({
    getUsers: build.query<User[], void>({
      query: () => "/users",
      providesTags: (result) =>
        result
          ? [...result.map(({ id }) => ({ type: "User" as const, id })), { type: "User", id: "LIST" }]
          : [{ type: "User", id: "LIST" }],
    }),

    createUser: build.mutation<User, Omit<User, "id">>({
      query: (body) => ({ url: "/users", method: "POST", body }),
      invalidatesTags: [{ type: "User", id: "LIST" }],
    }),
  }),
});

export const { useGetUsersQuery, useCreateUserMutation } = usersApi;
```

## Rules

- One base API with `injectEndpoints` per feature — keeps a single cache and store slice.
- Type every endpoint: `query<ResultType, ArgType>`.
- Use `providesTags` / `invalidatesTags` for automatic cache refresh — avoid manual refetches.
- Never store server data you can cache in RTK Query inside a separate slice.

## Checklist

- [ ] Endpoints typed (`Result`, `Arg`)
- [ ] Tags provided and invalidated correctly
- [ ] Auth header set in `prepareHeaders`
- [ ] Auto-generated hooks exported
