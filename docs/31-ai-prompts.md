# AI Prompts

## Purpose

This chapter defines how to **prompt AI assistants** to generate code that conforms to this handbook: prompt structure, required context, and reusable templates (which live in `/prompts`).

## Why It Matters

The quality of AI output is bounded by the quality of the prompt. A vague prompt yields generic, off-standard code; a precise, context-rich prompt yields senior-level, project-conformant code on the first try. Standardizing prompts makes AI a reliable contributor instead of a random one.

## Core Concepts

**Context first.** Give the AI the relevant handbook chapters, files, and conventions before the task.

**Constrain the stack.** State the approved libraries explicitly so the AI doesn't reach for alternatives.

**Specify the shape.** Define inputs, outputs, states, and file placement.

**Demand the standard.** Require typing, accessibility, and full state handling.

## Rules

1. Every prompt states the task, the relevant stack, and the expected output shape.
2. Reference the applicable handbook chapters as constraints.
3. Require full typing, accessibility, and loading/error/empty state handling.
4. Specify where files go (feature/folder) per chapters 06–08.
5. Ask the AI to explain non-trivial decisions.
6. Use the shared templates in `/prompts` for consistency.

## Best Practices

- Provide real file excerpts, not paraphrases.
- Iterate: review output against the checklist, then refine the prompt.
- Keep a library of proven prompts in `/prompts`.

## Bad Examples

```
❌ Vague, context-free prompt.
"Make a users page."
```
Result: untyped, off-stack, no states, wrong folder.

## Good Examples

```
✅ Precise, constrained, context-rich prompt.
Create a Users page for this project.

Stack (mandatory): React 19 + TypeScript, RTK Query for data, shadcn/ui,
Tailwind, React Router. No other libraries.

Requirements:
- Fetch users via a typed RTK Query endpoint (features/users/usersApi.ts).
- Handle loading (skeleton), error (retryable), and empty states.
- Presentational UserTable component; page composes it.
- Accessible, typed props, no `any`.
- Place files under features/users/ per the folder-structure chapter.

Explain any non-obvious decision in a short comment.
```

**Real World Example:** The good prompt names the stack, states, and folder — the AI produces conformant code in one pass.

**Production Example:** Because states and accessibility are required up front, the output ships without a rework cycle.

**Explanation:** Precise, constrained, context-rich prompts turn AI into a predictable senior contributor.

## Common Mistakes

- One-line prompts with no context or constraints.
- Not naming the approved stack (AI invents libraries).
- Omitting state/accessibility requirements.
- Not specifying file placement.

## AI Instructions

- Treat handbook references in a prompt as hard constraints.
- If context is missing, ask for it rather than guessing.
- Always deliver typed, accessible code with full state handling.
- Place files per the folder-structure chapter.

## Checklist

- [ ] Does the prompt state task, stack, and output shape?
- [ ] Are relevant chapters referenced as constraints?
- [ ] Are typing, a11y, and states required?
- [ ] Is file placement specified?

## Summary

Great AI output starts with a great prompt: context first, stack constrained, output shape specified, and standards (typing, accessibility, states) demanded. The reusable templates in `/prompts` operationalize this so AI reliably produces handbook-conformant code.

## 🔗 Related Chapters

Read next:

- 02-ai-development-rules.md
- 32-final-checklist.md
- 30-best-practices.md
