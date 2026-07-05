# Refactor Prompt

Improve code structure without changing behavior.

---

```
Refactor the following code. Behavior MUST stay identical.

CODE:
<paste code>

GOALS:
- Apply Clean Code, SOLID, DRY, KISS (docs/03–05).
- Extract reusable logic into hooks/utils; split oversized components.
- Improve naming and types; remove `any` and dead code.
- Align with the feature-based structure (docs/07).
- Reduce unnecessary re-renders / duplication where present.

CONSTRAINTS:
- No behavior or public API changes unless explicitly requested.
- Keep or add tests to prove behavior is preserved.
- Refactor in small, reviewable steps.

OUTPUT:
- The refactored files (full paths).
- A short list of what changed and why.
- Confirmation that tests still pass.
```
