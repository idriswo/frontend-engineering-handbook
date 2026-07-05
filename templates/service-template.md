# Service Template

A service module that isolates external I/O (HTTP, storage) behind a typed interface.
See [16-axios.md](../docs/16-axios.md) and [04-solid-principles.md](../docs/04-solid-principles.md).

## Axios client

```ts
// lib/http.ts
import axios from "axios";

export const http = axios.create({
  baseURL: import.meta.env.VITE_API_URL,
  timeout: 15000,
});

http.interceptors.request.use((config) => {
  const token = localStorage.getItem("accessToken");
  if (token) config.headers.Authorization = `Bearer ${token}`;
  return config;
});
```

## Service

```ts
// features/users/services/userService.ts
import { http } from "@/lib/http";
import type { User } from "../types/user.types";

export const userService = {
  async list(): Promise<User[]> {
    const { data } = await http.get<User[]>("/users");
    return data;
  },

  async getById(id: string): Promise<User> {
    const { data } = await http.get<User>(`/users/${id}`);
    return data;
  },

  async create(payload: Omit<User, "id">): Promise<User> {
    const { data } = await http.post<User>("/users", payload);
    return data;
  },
};
```

## Rules

- Services return **domain types**, not raw Axios responses.
- Centralize base URL, headers, and interceptors in one client.
- No React or UI code inside services — keep them framework-agnostic and testable.
- Prefer RTK Query for cached server state; use plain services for one-off/imperative calls.

## Checklist

- [ ] Typed inputs and return values
- [ ] Errors propagated (not swallowed) for the caller to handle
- [ ] No UI/React dependencies
- [ ] Reuses the shared HTTP client
