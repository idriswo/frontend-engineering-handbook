# JavaScript Glossary

Core JavaScript concepts every frontend engineer must know. See [glossary/terms.md](terms.md).

---

## Closure
A function that retains access to variables from its outer (lexical) scope even after that scope has returned.

## Hoisting
JavaScript moving `var` and function **declarations** to the top of their scope during compilation. `let`/`const` are hoisted but not initialized (temporal dead zone).

## Scope
The region where a binding is accessible: global, function, or block (`let`/`const`).

## `this`
A runtime binding determined by how a function is called. Arrow functions capture `this` from their enclosing scope.

## Prototype
The object a value delegates to for properties it doesn't own — the basis of JS inheritance.

## Event Loop
The mechanism that runs the call stack, then drains microtasks (promises), then macrotasks (timers, events) — enabling non-blocking async.

## Promise
An object representing the eventual result of an async operation: `pending` → `fulfilled` / `rejected`.

## `async` / `await`
Syntax sugar over promises that lets you write asynchronous code in a synchronous style.

## Destructuring
Extracting values from arrays/objects into variables: `const { id } = user`.

## Spread / Rest
`...` — spread expands an iterable/object; rest collects remaining items into an array/object.

## Immutability
Not mutating existing data but producing new copies — central to Redux and predictable React state.

## Truthy / Falsy
Falsy values: `false`, `0`, `""`, `null`, `undefined`, `NaN`. Everything else is truthy.

## Optional Chaining / Nullish Coalescing
`?.` safely accesses possibly-null paths; `??` provides a fallback only for `null`/`undefined`.

## Pure Function
A function with no side effects that returns the same output for the same input.
