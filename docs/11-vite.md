# Vite

## Purpose

This chapter defines how we use **Vite** as the build tool and dev server: configuration, environment variables, path aliases, and production builds.

## Why It Matters

Vite is the foundation of the developer experience — instant HMR, fast cold starts, and optimized production bundles. Correct configuration keeps builds fast and reproducible, keeps secrets safe, and enables clean imports via aliases. Misconfiguration leaks env variables, slows builds, or breaks production output.

## Core Concepts

**Dev vs. build.** Vite serves ES modules in dev (native, fast) and bundles with Rollup for production.

**Env variables.** Only variables prefixed `VITE_` are exposed to client code via `import.meta.env`. Everything else stays server-side.

**Path aliases.** `@` maps to `src/` for clean absolute imports.

**Code splitting.** Vite splits routes lazily loaded with `React.lazy` automatically.

## Rules

1. Only expose client config through `VITE_`-prefixed variables.
2. Never put secrets (API keys with privileges) in `VITE_` variables — they ship to the browser.
3. Use the `@` alias for `src/` imports; avoid deep `../../..` paths.
4. Keep `vite.config.ts` typed and minimal.
5. Validate required env variables at startup.

## Best Practices

- Define env types in `vite-env.d.ts` for autocomplete and safety.
- Use `.env.local` for machine-specific secrets (git-ignored).
- Configure manual chunks only when bundle analysis justifies it.

## Bad Examples

```ts
// ❌ Secret exposed to the browser; relative import hell.
const key = import.meta.env.VITE_STRIPE_SECRET_KEY; // ships to client!
import { api } from "../../../../shared/lib/api";
```

## Good Examples

```ts
// vite.config.ts
import { defineConfig } from "vite";
import react from "@vitejs/plugin-react";
import path from "node:path";

export default defineConfig({
  plugins: [react()],
  resolve: {
    alias: { "@": path.resolve(__dirname, "./src") },
  },
});
```

```ts
// src/vite-env.d.ts — typed env
interface ImportMetaEnv {
  readonly VITE_API_URL: string;
}
interface ImportMeta {
  readonly env: ImportMetaEnv;
}
```

```ts
// ✅ Clean alias import, safe public variable.
import { api } from "@/shared/lib/api";
const apiUrl = import.meta.env.VITE_API_URL;
```

**Real World Example:** `@/shared/lib/api` stays stable even when files move; relative paths would break.

**Production Example:** Typing `ImportMetaEnv` makes a missing `VITE_API_URL` a compile-time error rather than a runtime `undefined`.

**Explanation:** Aliases plus typed env give safe, refactor-proof configuration.

## Common Mistakes

- Putting privileged secrets in `VITE_` variables.
- Deep relative imports instead of `@`.
- Forgetting to type `import.meta.env`.
- Committing `.env.local`.

## AI Instructions

- Use the `@` alias for all `src/` imports.
- Only reference `VITE_`-prefixed public variables in client code.
- Never place privileged secrets in client env.
- Keep `vite.config.ts` typed and minimal.

## Checklist

- [ ] Are all client env variables `VITE_`-prefixed and non-secret?
- [ ] Is `import.meta.env` typed?
- [ ] Are imports using the `@` alias?
- [ ] Is `.env.local` git-ignored?

## Summary

Vite gives fast dev and optimized builds. We expose only `VITE_` public config, keep secrets server-side, use the `@` alias for clean imports, and type env variables so misconfiguration fails at compile time.

## 🔗 Related Chapters

Read next:

- 07-folder-structure.md
- 24-performance.md
- 30-best-practices.md
