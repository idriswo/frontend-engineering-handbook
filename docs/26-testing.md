# Testing

## Purpose

This chapter defines how we test the frontend with **Vitest** and **React Testing Library (RTL)**: what to test, how to write behavior-focused tests, and how to keep tests fast and meaningful.

## Why It Matters

Tests are the safety net that lets us change code confidently. Good tests catch regressions, document behavior, and enable refactoring. Bad tests (testing implementation details) break on every change and provide false confidence. The goal is tests that verify user-visible behavior and survive refactors.

## Core Concepts

**Test behavior, not implementation.** Assert what the user sees and does, not internal state.

**RTL queries by accessibility.** Prefer `getByRole`/`getByLabelText` — this also validates accessibility.

**Arrange–Act–Assert.** Render, interact, assert observable outcome.

**Mock at the boundary.** Mock the network (MSW) or the API layer, not internal functions.

## Rules

1. Test user-visible behavior, not internal implementation.
2. Query by accessible roles/labels first; avoid `test-id` unless necessary.
3. Cover the four async states (loading, error, empty, success) for data views.
4. Mock at the network/API boundary; keep tests deterministic.
5. Keep tests isolated and fast; no shared mutable state between tests.
6. Every bug fix adds a regression test.

## Best Practices

- Use `userEvent` for realistic interactions.
- Co-locate tests with the code (`Component.test.tsx`).
- Use MSW to mock API responses consistently across tests.

## Bad Examples

```tsx
// ❌ Tests implementation detail (state), brittle, not user-facing.
test("sets loading true", () => {
  const { result } = renderHook(() => useThing());
  expect(result.current.internalLoadingFlag).toBe(true);
});
```

## Good Examples

```tsx
import { render, screen } from "@testing-library/react";
import userEvent from "@testing-library/user-event";
import { LoginForm } from "./LoginForm";

test("shows validation error for invalid email", async () => {
  const onLogin = vi.fn();
  render(<LoginForm onLogin={onLogin} />);

  await userEvent.type(screen.getByRole("textbox", { name: /email/i }), "bad");
  await userEvent.click(screen.getByRole("button", { name: /log in/i }));

  expect(await screen.findByRole("alert")).toHaveTextContent(/invalid email/i);
  expect(onLogin).not.toHaveBeenCalled();
});
```

**Real World Example:** The test drives the form like a user and asserts the visible error — it survives internal refactors.

**Production Example:** Because it queries by role/label, the test also fails if the form loses accessible labels.

**Explanation:** Behavior-focused, accessibility-first tests catch real regressions and double as a11y checks.

## Common Mistakes

- Testing internal state/implementation details.
- Over-using `test-id` instead of accessible queries.
- Non-deterministic tests from unmocked network.
- Not testing error/empty states.

## AI Instructions

- Write behavior-focused tests with RTL.
- Query by accessible role/label first.
- Mock at the network/API boundary (MSW).
- Cover loading, error, empty, and success states.

## Checklist

- [ ] Does the test assert user-visible behavior?
- [ ] Are accessible queries used?
- [ ] Are async states covered?
- [ ] Is the network mocked at the boundary?
- [ ] Is the test deterministic and isolated?

## Summary

We test with Vitest + React Testing Library, focusing on user-visible behavior and accessible queries. Tests mock at the boundary, cover all async states, survive refactors, and grow with every bug fix — providing real, durable confidence.
