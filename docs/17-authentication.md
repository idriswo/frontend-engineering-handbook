# Authentication

## Purpose

This chapter defines the **authentication and authorization** model: JWT access/refresh tokens, an auth provider, protected routes, route guards, and role-based access control (RBAC).

## Why It Matters

Auth is security-critical. A weak implementation leaks tokens, exposes private routes, or grants excessive privileges. A clear, centralized auth model protects users and data while keeping the rest of the app simple — components just ask "is the user allowed?" and never manage tokens themselves.

## Core Concepts

**Access + refresh tokens.** Short-lived access token (JWT) for requests; long-lived refresh token to obtain new access tokens.

**Auth provider/slice.** Central owner of session state (user, token, status).

**Protected routes / guards.** Route wrappers that redirect unauthenticated users.

**RBAC.** Access decisions based on the user's roles/permissions.

## Rules

1. Store the access token in memory/Redux; store refresh tokens securely (httpOnly cookie preferred over `localStorage`).
2. Attach tokens via the Axios interceptor, never manually.
3. Refresh access tokens centrally on 401; log out on refresh failure.
4. Protect every private route with a guard.
5. Enforce role checks for privileged UI and routes.
6. Never trust the client for authorization — the server is the source of truth; client checks are UX, not security.

## Best Practices

- Prefer httpOnly, `Secure`, `SameSite` cookies for refresh tokens.
- Restore the session on app load (see `restoreSession` in chapter 15).
- Centralize permission checks in a `useAuth`/`usePermissions` hook.

## Bad Examples

```tsx
// ❌ Token in localStorage read everywhere, no guard, client-only "security".
if (localStorage.getItem("token")) {
  return <AdminPanel />; // anyone can set this key
}
```

## Good Examples

```tsx
// features/auth/ProtectedRoute.tsx
import { Navigate } from "react-router-dom";
import { useAppSelector } from "@/app/hooks";

interface ProtectedRouteProps {
  children: React.ReactNode;
  requiredRole?: Role;
}

export function ProtectedRoute({ children, requiredRole }: ProtectedRouteProps) {
  const { user, status } = useAppSelector((state) => state.auth);

  if (status === "loading") return <FullPageSpinner />;
  if (!user) return <Navigate to="/login" replace />;
  if (requiredRole && !user.roles.includes(requiredRole)) {
    return <Navigate to="/forbidden" replace />;
  }

  return <>{children}</>;
}
```

**Real World Example:** Wrapping `<AdminPanel />` in `<ProtectedRoute requiredRole="admin">` centralizes the access rule.

**Production Example:** On 401 the interceptor refreshes silently; if refresh fails the user is logged out once, cleanly.

**Explanation:** Centralized session state + guards + interceptor refresh make auth consistent and hard to bypass in the UI.

## Common Mistakes

- Trusting client-side role checks for real security.
- Storing refresh tokens in `localStorage` (XSS-exposed).
- Manually attaching tokens per request.
- Unprotected private routes.

## AI Instructions

- Route all auth state through the auth slice/provider.
- Guard private routes; enforce role checks via the guard.
- Never store privileged secrets in `localStorage` when a cookie is available.
- Treat client checks as UX only; assume the server enforces authorization.

## Checklist

- [ ] Are private routes guarded?
- [ ] Are role checks enforced for privileged UI?
- [ ] Are tokens attached via interceptor and refreshed centrally?
- [ ] Are refresh tokens stored securely?
- [ ] Is the session restored on load?

## Summary

Authentication is centralized around access/refresh JWTs, an auth slice, route guards, and RBAC. The client enforces UX-level checks while the server remains the security authority. Tokens are handled by interceptors, never by components.
