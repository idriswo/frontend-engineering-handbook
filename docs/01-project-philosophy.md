# Project Philosophy

## Purpose

This chapter defines the **engineering philosophy** that governs every decision in this codebase. It is the foundation on which all other chapters stand. Before a single line of code is written, every contributor — junior, senior, team lead, or AI assistant — must understand *why* this project exists, *how* we make trade-offs, and *what* "good" means here.

A philosophy is not a style guide. A style guide tells you where to put a semicolon. A philosophy tells you how to reason when the style guide is silent.

## Why It Matters

Software projects rarely fail because of a missing framework. They fail because of **inconsistent decision-making at scale**. When ten engineers each optimize for a different value — one for speed, one for cleverness, one for premature abstraction — the codebase drifts into entropy.

A shared philosophy removes debate about settled questions, lets new engineers become productive in days, makes AI generate code that fits the project instead of fighting it, and ensures the codebase still makes sense in three years.

## Core Concepts

**Clarity over cleverness.** Code is read far more often than written. Optimize for the reader with zero context.

**Boring is a feature.** Proven, documented patterns over novel ones. Novelty is a cost paid only when it buys real value.

**Explicit over implicit.** Hidden behavior, magic globals, and implicit coupling are debt. State intentions in code.

**Locality of behavior.** Things that change together should live together. A developer should not open eight files to understand one feature.

**The codebase is a product.** Its users are engineers. Developer experience is a first-class requirement.

## Rules

1. Every technical decision must be justifiable in one sentence a junior can understand.
2. When two solutions are equally correct, choose the simpler one.
3. No abstraction is introduced before it is needed **twice**.
4. Consistency with the existing codebase beats personal preference.
5. Performance, accessibility, and security are requirements, not polish.
6. Every feature must handle loading, error, empty, and success states.

## Best Practices

- Read the surrounding code before adding to it; match its idioms.
- Write the code you would want to inherit.
- Prefer deleting code over adding code.
- Document the *why*, not the *what*.

## Bad Examples

```tsx
// ❌ Clever, implicit, unreadable, unsafe.
const p = (u: any) => u?.r?.filter((x: any) => x.a).map((x: any) => x.n);
```

Single-letter names erase meaning; `any` erases safety.

## Good Examples

```tsx
// ✅ Explicit, typed, self-documenting.
interface Role {
  name: string;
  isActive: boolean;
}

interface User {
  roles: Role[];
}

const getActiveRoleNames = (user: User): string[] =>
  user.roles.filter((role) => role.isActive).map((role) => role.name);
```

**Real World Example:** A reviewer approves `getActiveRoleNames` in seconds; the first version triggers a review thread.

**Production Example:** In a dashboard rendering thousands of users, the typed version lets TypeScript catch a renamed field at compile time — before production.

**Explanation:** Naming reveals intent, types enforce contracts, and the reviewer never asks "what does this do?"

## Common Mistakes

- Introducing abstractions "for a future" that never arrives (violates YAGNI).
- Copy-pasting tutorial patterns without understanding them.
- Treating consistency as optional when inconvenient.
- Confusing "shorter" with "simpler."

## AI Instructions

- Prefer explicit, typed, readable code over compact code.
- Never introduce an abstraction unless a duplication already exists.
- Match the naming and structure of surrounding files.
- Never use `any` to bypass a type error — solve the type.
- Justify non-obvious decisions in a short comment.

## Checklist

- [ ] Can a junior understand this without asking questions?
- [ ] Is this the simplest correct solution?
- [ ] Does it match existing codebase conventions?
- [ ] Are performance, accessibility, and security respected?
- [ ] Would I be happy to inherit this code?

## Summary

The philosophy of this project is **clarity, consistency, and intentional simplicity**. We write code for the humans and AIs who read it next. Every other chapter is an application of these principles to a specific technology or problem.
