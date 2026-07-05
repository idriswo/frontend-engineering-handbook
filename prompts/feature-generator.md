# Feature Generator Prompt

Scaffold a complete, self-contained feature module.

---

```
Generate a complete feature module named "<featureName>" following docs/06 and docs/07.

FUNCTIONALITY:
<describe what the feature does>

STRUCTURE (create all that apply):
features/<featureName>/
├── api/        RTK Query endpoints (typed query/mutation, tags)
├── components/ presentational + container components
├── hooks/      feature-specific hooks
├── types/      TypeScript types
├── utils/      pure helpers
└── index.ts    public API (export only what other modules may use)

RULES:
- No deep imports into other features — go through their index.ts.
- Typed endpoints with providesTags/invalidatesTags.
- Handle loading/error/empty states in UI.
- Forms use React Hook Form + Zod.
- Include unit tests for hooks/utils.

OUTPUT every file with its full path and complete contents.
```

See the [feature template](../templates/feature-template.md).
