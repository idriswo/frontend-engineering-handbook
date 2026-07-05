# Axios

## Purpose

This chapter defines how we configure **Axios**: a single shared instance, interceptors for auth and errors, and its role as the transport layer beneath RTK Query.

## Why It Matters

A centralized HTTP client is the single place to attach auth tokens, handle refresh, normalize errors, and set base configuration. Scattering raw `axios.get` calls across components duplicates this logic and makes cross-cutting concerns (auth, retries, logging) impossible to manage.

## Core Concepts

**Single instance.** One configured `axios` instance (`api`) with `baseURL`, timeout, and headers.

**Request interceptor.** Attaches the auth token to every outgoing request.

**Response interceptor.** Normalizes errors and triggers token refresh on 401.

**Transport for RTK Query.** RTK Query's base query wraps this instance so all traffic shares the same pipeline.

## Rules

1. Create one shared Axios instance; never call `axios` directly in components.
2. Attach auth tokens via a request interceptor, not manually per call.
3. Handle 401 refresh centrally in a response interceptor.
4. Normalize errors into a consistent shape before they reach the UI.
5. Read the base URL from typed env (`VITE_API_URL`).

## Best Practices

- Keep the instance in `shared/lib/api.ts`.
- Guard against infinite refresh loops with a retry flag.
- Type request/response payloads at the call site (via RTK Query or services).

## Bad Examples

```tsx
// ❌ Direct axios in a component, manual token, no error normalization.
axios.get("https://api.example.com/users", {
  headers: { Authorization: `Bearer ${localStorage.getItem("token")}` },
});
```

## Good Examples

```ts
// shared/lib/api.ts
import axios from "axios";
import { store } from "@/app/store";
import { logout } from "@/features/auth/authSlice";

export const api = axios.create({
  baseURL: import.meta.env.VITE_API_URL,
  timeout: 15_000,
});

api.interceptors.request.use((config) => {
  const token = store.getState().auth.token;
  if (token) config.headers.Authorization = `Bearer ${token}`;
  return config;
});

api.interceptors.response.use(
  (response) => response,
  async (error) => {
    if (error.response?.status === 401) {
      store.dispatch(logout());
    }
    return Promise.reject(normalizeError(error));
  }
);
```

**Real World Example:** Every request gets the token automatically; no component ever touches headers.

**Production Example:** A 401 anywhere triggers one central logout path instead of scattered handling.

**Explanation:** Centralizing transport concerns keeps auth, errors, and config in exactly one place.

## Common Mistakes

- Calling `axios` directly in components.
- Manually attaching tokens per request.
- No error normalization, so the UI handles raw Axios errors.
- Refresh loops from missing a retry guard.

## AI Instructions

- Use the shared `api` instance only; never `axios` directly in components.
- Rely on interceptors for auth and error handling.
- Normalize errors before they reach the UI.
- Read the base URL from typed env.

## Checklist

- [ ] Is there a single shared Axios instance?
- [ ] Are tokens attached via interceptor?
- [ ] Is 401/refresh handled centrally?
- [ ] Are errors normalized?
- [ ] No direct `axios` in components?

## Summary

Axios is the centralized transport layer: one instance, request/response interceptors for auth and error normalization, and integration beneath RTK Query. Components never touch Axios directly — cross-cutting concerns live in one place.
