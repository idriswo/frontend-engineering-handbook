# Code Review Prompt

Review a diff/PR against the handbook standards.

---

```
Review the following code as a senior frontend engineer.

CODE / DIFF:
<paste code or diff>

REVIEW FOR:
1. Correctness — bugs, edge cases, unhandled loading/error/empty states.
2. Architecture — feature structure, separation of concerns, no cross-feature deep imports.
3. Types — no `any`, correct generics, proper narrowing.
4. Clean code & SOLID/DRY/KISS/YAGNI — duplication, complexity, naming (docs/03–08).
5. React — hooks rules, derived vs synced state, stable keys, unnecessary re-renders.
6. Performance & Security — see checklists/performance.md and checklists/security.md.
7. Tests — coverage of the changed logic.

OUTPUT:
- Findings grouped by severity (Blocker / Major / Minor / Nit).
- For each: file:line, the problem, and a concrete suggested fix.
- Call out what's done well.
- End with an overall verdict: approve / request changes.
```
