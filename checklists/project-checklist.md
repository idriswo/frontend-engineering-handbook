# ✅ Project Setup Checklist

For bootstrapping a new project on this handbook's standards. Reference: [11-vite.md](../docs/11-vite.md), [06-project-architecture.md](../docs/06-project-architecture.md).

## Scaffolding

- [ ] Vite + React + TypeScript project created
- [ ] Feature-based folder structure in place — see [07-folder-structure.md](../docs/07-folder-structure.md)
- [ ] Path aliases configured (`@/…`) in `tsconfig` + Vite
- [ ] `.env.example` created; `.env` gitignored

## Tooling

- [ ] ESLint configured — see [27-eslint.md](../docs/27-eslint.md)
- [ ] Prettier configured — see [28-prettier.md](../docs/28-prettier.md)
- [ ] Husky + lint-staged pre-commit hooks — see [29-husky.md](../docs/29-husky.md)
- [ ] TypeScript `strict` mode enabled
- [ ] Testing set up (Vitest + Testing Library)

## Core Stack

- [ ] Redux Toolkit store + RTK Query base API wired
- [ ] React Router with layout routes and guards
- [ ] Tailwind CSS + shadcn/ui installed and themed
- [ ] Axios client with interceptors (if used alongside RTK Query)
- [ ] React Hook Form + Zod for forms/validation

## Foundations

- [ ] Global error boundary and 404 page
- [ ] Auth flow scaffolded (login, protected routes, token refresh)
- [ ] Theme provider (dark/light)
- [ ] CI pipeline (build + lint + test) running on PRs
- [ ] README with setup, scripts, and conventions

---

**Rule of thumb:** get the guardrails (lint, types, hooks, CI) in place before writing features.
