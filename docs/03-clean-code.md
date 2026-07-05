# Clean Code

## Purpose

This chapter defines what **clean code** means in this project and how to write it consistently. Clean code is not an aesthetic preference — it is an economic decision that reduces the cost of change over the lifetime of the software.

## Why It Matters

Most of an engineer's time is spent *reading* code, not writing it. Every hour saved in comprehension multiplies across the team and the life of the project. Clean code reduces onboarding time, lowers bug rates during changes, speeds reviews, and lets AI extend code correctly. Messy code compounds like debt until a module becomes untouchable.

## Core Concepts

**Meaningful names.** Names reveal intent. `getActiveUsers` beats `getData`.

**Small functions.** A function does one thing at one level of abstraction.

**Few arguments.** Prefer zero to three; group related ones into an object.

**No hidden side effects.** A function named `isValid` must not mutate state.

**Comments explain *why*, never *what*.** If a comment is needed to explain what the code does, rename or refactor first.

## Rules

1. Names must be descriptive, pronounceable, and searchable.
2. Functions should be short and do exactly one thing.
3. Maximum three positional arguments; use an options object beyond that.
4. No magic numbers or strings — extract named constants.
5. Delete dead code; do not comment it out.
6. Keep nesting shallow — prefer early returns over deep `if` pyramids.

## Best Practices

- Prefix booleans with `is`, `has`, `should`, `can`.
- Extract complex conditionals into named variables or functions.
- Keep files focused: one primary responsibility per file.
- Favor pure functions — trivial to test and reason about.

## Bad Examples

```tsx
// ❌ Vague names, magic numbers, deep nesting.
function proc(d: any[]) {
  let r = [];
  for (let i = 0; i < d.length; i++) {
    if (d[i].s === 1) {
      if (d[i].a > 18) {
        r.push(d[i]);
      }
    }
  }
  return r;
}
```

## Good Examples

```tsx
// ✅ Intent-revealing, typed, flat, no magic values.
const ACTIVE_STATUS = 1;
const ADULT_AGE = 18;

interface Member {
  status: number;
  age: number;
}

const isActiveAdult = (member: Member): boolean =>
  member.status === ACTIVE_STATUS && member.age >= ADULT_AGE;

const getActiveAdultMembers = (members: Member[]): Member[] =>
  members.filter(isActiveAdult);
```

**Real World Example:** A reviewer instantly understands `getActiveAdultMembers` — no decoding of `proc`.

**Production Example:** When adult age becomes 21, you edit one named constant instead of hunting magic numbers.

**Explanation:** Extracting `isActiveAdult` and naming constants turns cryptic logic into a readable business rule.

## Common Mistakes

- Abbreviating names to save keystrokes (`usr`, `btn`, `hdlr`).
- Functions that do three things "to save a file."
- Leaving commented-out code "just in case" — that is what git is for.
- Explaining bad code with comments instead of fixing it.

## AI Instructions

- Generate intent-revealing names for every symbol.
- Keep functions small and single-purpose.
- Extract magic numbers/strings into named constants.
- Use early returns to keep nesting shallow.
- Never leave dead or commented-out code.

## Checklist

- [ ] Do all names clearly reveal intent?
- [ ] Does each function do exactly one thing?
- [ ] Any magic numbers or strings left?
- [ ] Is nesting shallow (early returns used)?
- [ ] Any dead or commented-out code to remove?

## Summary

Clean code is optimized for the reader. Through meaningful names, small single-purpose functions, named constants, and shallow nesting, we make the codebase cheap to change — a continuous discipline applied in every commit and review.
