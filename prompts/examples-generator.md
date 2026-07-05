# Examples Generator Prompt

Generate a real-world usage example for the `examples/` folder.

---

```
Create a complete, real-world example for "<topic>" (e.g. auth, forms, rtk-query).

FORMAT (Markdown for examples/<topic>/README.md):
# <Topic> Example
Short intro: what it demonstrates and which docs chapter it maps to.

## Scenario
A concrete, realistic use case.

## Code
Full, runnable code with file paths. Typed, handbook-compliant.

## Explanation
Walk through the key decisions and patterns.

## Related
Links to relevant docs/ chapters and templates/.

RULES:
- Production-grade, not toy code.
- Handle loading/error/empty states where relevant.
- Reuse the handbook stack and conventions.
```
