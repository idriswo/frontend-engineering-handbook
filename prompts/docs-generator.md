# Docs Generator Prompt

Generate a new handbook chapter matching the existing `docs/` style.

---

```
Write a handbook chapter for "<topic>" matching the exact structure of the existing
docs/*.md chapters.

USE THESE SECTIONS (in order):
# <Topic>
## Purpose
## Why It Matters
## Core Concepts
## Rules            (numbered)
## Best Practices
## Bad Examples     (```tsx with ❌ comments)
## Good Examples    (```tsx with ✅ comments)
## Common Mistakes
## AI Instructions
## Checklist        (- [ ] items)
## Summary
## 🔗 Related Chapters

REQUIREMENTS:
- Stack-accurate (React 19, TS, RTK Query, Tailwind, shadcn, RHF/Zod).
- Concise, opinionated, production-focused — no filler.
- Code examples must be valid and runnable.
- Link related chapters by filename.
```
