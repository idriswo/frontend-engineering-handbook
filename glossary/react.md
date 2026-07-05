# React Glossary

Core React terms used throughout this handbook. See [09-react.md](../docs/09-react.md).

---

## Component
A reusable, self-contained piece of UI declared as a function that returns JSX. We use **functional components only**.

## Props
Read-only inputs passed from parent to child. Never mutate props; treat them as immutable.

## State
Data owned and managed by a component that, when changed, triggers a re-render (`useState`, `useReducer`).

## Hook
A function starting with `use` that lets components access state and lifecycle features. Must be called unconditionally at the top level.

## Derived State
A value computed from props/state during render instead of stored separately. Prefer this over syncing with `useEffect`.

## Effect
`useEffect` — runs side effects to synchronize a component with **external systems** (subscriptions, DOM, network). Not for deriving values.

## Reconciliation
React's diffing process that compares the new element tree to the previous one to compute minimal DOM updates.

## Key
A stable, unique identifier on list items that helps reconciliation track elements. Never use array index on mutable lists.

## Controlled Component
A form element whose value is driven by React state, making React the single source of truth.

## Lifting State Up
Moving shared state to the closest common ancestor so multiple children can read/update it.

## Context
`createContext` + `Provider` — passes data through the tree without prop drilling. Use for low-frequency cross-cutting state.

## Suspense
A boundary that shows a fallback while lazily-loaded components or data are loading.

## Error Boundary
A component that catches render-time errors in its subtree and shows a fallback UI. See [25-error-handling.md](../docs/25-error-handling.md).
