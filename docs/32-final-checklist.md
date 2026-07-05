# Final Checklist

## Purpose

This chapter is the **master checklist** for the entire handbook — a single verification pass applied before any feature is considered done. It cross-references every chapter so nothing is missed.

## Why It Matters

Standards only work if they're verified. A final checklist turns the whole handbook into an actionable gate: instead of remembering thirty-one chapters, an engineer or AI runs one list. It is the last line of defense against shipping code that violates the project's standards.

## Core Concepts

**Definition of Done.** A feature is complete only when it passes this checklist, not merely when it "works."

**Cross-cutting verification.** The list spans architecture, code quality, state, forms, styling, performance, errors, and tooling.

**Applies to humans and AI.** Every contributor runs it before opening a PR.

## Rules

1. No feature merges until this checklist passes.
2. Any unchecked item is a blocker, not a nice-to-have.
3. AI-generated code is verified against this list before review.
4. The reviewer re-verifies the critical items during review.

## Best Practices

- Copy this list into the PR description and check items off.
- Automate what you can (lint, format, type-check, tests) via hooks and CI.
- Treat repeated failures on an item as a signal to improve tooling or docs.

## Bad Examples

```
❌ "It renders on my machine, shipping it."
No states, untyped, unstyled tokens, no tests, wrong folder.
```

## Good Examples

```
✅ PR description includes the completed checklist below,
with lint/type-check/tests green and all states handled.
```

**Real World Example:** A reviewer sees the completed checklist and focuses on logic, not on catching missing states.

**Production Example:** The feature ships with loading/error/empty states, tests, and accessibility already verified — no post-merge hotfix.

**Explanation:** One authoritative list operationalizes the entire handbook into a repeatable gate.

## The Master Checklist

**Philosophy & Architecture (01, 06, 07)**
- [ ] Simplest correct solution; no over-engineering.
- [ ] Organized by feature; business logic out of UI.
- [ ] Files placed in the correct feature/shared/app folders.

**Code Quality (03, 04, 05, 08, 10)**
- [ ] Clean, intent-revealing names; small single-purpose functions.
- [ ] SOLID respected; no god components.
- [ ] DRY/KISS/YAGNI balanced (rule of three).
- [ ] Naming conventions followed.
- [ ] Fully typed; no unjustified `any`/`as`.

**React & State (09, 13, 14, 15)**
- [ ] Functional components, correct hooks, stable keys.
- [ ] Server state via RTK Query; global client state via Redux only when needed.
- [ ] `createAsyncThunk` only for client-state async flows.

**Networking & Auth (16, 17)**
- [ ] All HTTP via the shared Axios instance/RTK Query; no direct calls in components.
- [ ] Private routes guarded; roles enforced; tokens handled by interceptors.

**Forms & Validation (18, 19)**
- [ ] Forms use React Hook Form + Zod; types inferred from schemas.
- [ ] External data validated at the boundary.

**Styling & UI (20, 21, 22, 23)**
- [ ] Tailwind tokens + `cn()`; no inline styles.
- [ ] Accessible shadcn/ui primitives; Radix behavior preserved.
- [ ] Animations performant + reduced-motion aware.
- [ ] Icons named-imported, sized, and labelled/hidden correctly.

**Performance & Errors (24, 25)**
- [ ] Routes/heavy components lazy-loaded; memoization only where measured.
- [ ] Loading, error, empty, success states handled; error boundary in place.

**Tooling (11, 26, 27, 28, 29)**
- [ ] Env typed and secret-safe.
- [ ] Behavior-focused tests cover async states; bug fixes have regression tests.
- [ ] ESLint clean; Prettier formatted; Husky gates pass.

**AI (02, 30, 31)**
- [ ] AI output follows the approved stack and this handbook.
- [ ] Config/constants/theming centralized per best practices.

## Common Mistakes

- Treating "it works" as "it's done."
- Skipping the checklist under time pressure.
- Not automating the mechanical checks.

## AI Instructions

- Run this full checklist against any generated feature before presenting it.
- Report which items pass and flag any that don't.
- Never claim completion while items remain unchecked.

## Checklist

- [ ] Have all sections above been verified?
- [ ] Are lint, format, type-check, and tests green?
- [ ] Are all four async states handled everywhere?
- [ ] Is the feature accessible, secure, and performant?

## Summary

This final checklist is the Definition of Done for the whole handbook. Passing it — across architecture, quality, state, forms, styling, performance, errors, tooling, and AI — is what makes a feature truly complete. Run it every time, for every feature, human- or AI-written.
