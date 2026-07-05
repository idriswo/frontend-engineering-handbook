# AI Development Rules

## Purpose

This chapter defines how **AI coding assistants** (Claude, Copilot, Cursor, and others) must operate inside this codebase. AI is a first-class contributor held to the same engineering standard as any human engineer. These rules make AI output predictable, safe, and consistent with the rest of the handbook.

## Why It Matters

AI assistants are fast but context-blind by default. Without explicit rules they generate plausible-looking code that violates conventions, introduce libraries outside the approved stack, use `any`, skip error handling, and ignore accessibility. Disciplined rules turn AI from a liability into a force multiplier: AI-generated code should be **indistinguishable from senior-engineer code** and pass review on the first attempt.

## Core Concepts

**AI is a contributor, not an oracle.** Its output is reviewed like any human PR.

**Context is mandatory.** AI must read the relevant files, this handbook, and surrounding code before generating.

**The approved stack is a hard boundary.** AI may not introduce dependencies outside the documented list.

**Determinism over creativity.** In a codebase, consistency beats novelty.

## Rules

1. Always follow the structure and conventions in this handbook.
2. Never introduce a library outside the approved stack (chapters 09–29).
3. Never use `any` unless genuinely unavoidable — then document why.
4. Every component must be typed, accessible, and functional (no class components).
5. Every async operation must handle loading, error, and empty states.
6. Never hallucinate an API — if unsure, state the uncertainty.
7. Match file, folder, and naming conventions of the existing project.
8. Generate the *simplest correct* solution; never over-engineer.

## Best Practices

- Feed the AI the relevant handbook chapter before asking for code.
- Ask the AI to explain reasoning for non-trivial decisions.
- Review AI output for security (XSS, exposed secrets) and performance.
- Use the prompt templates in `/prompts` for consistent results.

## Bad Examples

```tsx
// ❌ any, no states, inline fetch, bypasses the networking layer.
function UserList() {
  const [data, setData] = useState<any>();
  useEffect(() => {
    fetch("/api/users").then((r) => r.json()).then(setData);
  }, []);
  return data.map((u: any) => <div>{u.name}</div>);
}
```

## Good Examples

```tsx
// ✅ Uses the project's RTK Query layer, typed, all states handled.
import { useGetUsersQuery } from "@/features/users/usersApi";

export function UserList() {
  const { data: users, isLoading, isError } = useGetUsersQuery();

  if (isLoading) return <UserListSkeleton />;
  if (isError) return <ErrorState message="Failed to load users." />;
  if (!users?.length) return <EmptyState message="No users found." />;

  return (
    <ul>
      {users.map((user) => (
        <li key={user.id}>{user.name}</li>
      ))}
    </ul>
  );
}
```

**Real World Example:** The good version reuses the RTK Query layer, so caching and refetching come for free.

**Production Example:** All three async states are handled, so it never crashes on slow networks or empty datasets.

**Explanation:** The AI followed the stack, typed everything, and handled every state — exactly what a senior engineer ships.

## Common Mistakes

- Accepting AI output without review because "it looks right."
- Letting the AI pick libraries instead of enforcing the stack.
- Forgetting to give the AI context.
- Allowing `any` to silence a type error.

## AI Instructions

- Read this handbook and relevant files before generating.
- Never deviate from the approved technology stack.
- Always handle loading, error, and empty states in async UI.
- Prefer editing existing files over parallel implementations.
- When uncertain, say so instead of inventing an API.

## Checklist

- [ ] Uses only approved-stack libraries?
- [ ] Everything typed (no unjustified `any`)?
- [ ] Loading, error, and empty states handled?
- [ ] Matches naming and folder conventions?
- [ ] Accessible, secure, and performant?

## Summary

AI is a full contributor bound by the same standards as human engineers. Approved stack only, full typing, complete state handling, and strict adherence to this handbook ensure AI-generated code is consistent, safe, and production-ready on the first pass.

## 🔗 Related Chapters

Read next:

- 01-project-philosophy.md
- 31-ai-prompts.md
- 32-final-checklist.md
