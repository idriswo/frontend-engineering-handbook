# Templates Generator Prompt

Generate a reusable code template for the `templates/` folder.

---

```
Create a reusable template for "<artifact>" (component, hook, page, layout,
feature, service, provider, or api slice).

FORMAT (Markdown for templates/<artifact>-template.md):
# <Artifact> Template
One-line purpose + link to the related docs chapter.

## Structure        (folder/file layout, if applicable)
## Template         (```tsx/ts full, runnable, typed boilerplate)
## Usage            (how to consume it, if applicable)
## Rules            (bullet list of constraints)
## Checklist        (- [ ] items)

RULES:
- Copy-paste ready and generic — placeholders clearly named.
- Strictly typed, handbook-compliant, no `any`.
- Follows the feature-based structure and naming conventions.
```
