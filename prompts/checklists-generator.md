# Checklists Generator Prompt

Generate a quality checklist for the `checklists/` folder.

---

```
Create a checklist for "<stage/topic>" (e.g. before-commit, performance, security).

FORMAT (Markdown for checklists/<name>.md):
# ✅ <Title> Checklist
One-line purpose + link to the related docs chapter.

## <Group 1>
- [ ] actionable, verifiable item
- [ ] ...

## <Group 2>
- [ ] ...

End with a **Rule of thumb:** one-liner.

RULES:
- Every item must be concrete and checkable (not vague advice).
- Group items logically (code, tests, security, ops…).
- Reference relevant docs/ chapters and other checklists.
- Keep it practical — the list should be usable in under a few minutes.
```
