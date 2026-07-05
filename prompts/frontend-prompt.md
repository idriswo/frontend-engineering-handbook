# Frontend Prompt

A general-purpose prompt for building any UI in this handbook's standards.

---

```
Build the following frontend using the handbook stack (React 19 + TS + RTK Query +
React Router + RHF/Zod + Tailwind + shadcn/ui).

TASK:
<describe the UI / feature>

REQUIREMENTS:
- Feature-based structure; place files under features/<name>/ with a public index.ts.
- Typed props and state; no `any`.
- Server data via RTK Query with tag-based caching; no ad-hoc fetch.
- Forms via React Hook Form + Zod schema validation.
- Handle loading, error, and empty states explicitly.
- Accessible and responsive (WCAG AA, mobile-first).
- Reuse shared components from src/components before creating new ones.

DELIVERABLES:
- All files with full paths.
- Types and Zod schemas.
- Tests for non-trivial logic.
- Short explanation of the structure.
```

---

**Tip:** prepend [master-prompt.md](master-prompt.md) for full rule enforcement.
