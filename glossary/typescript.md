# TypeScript Glossary

Core TypeScript terms. See [10-typescript.md](../docs/10-typescript.md) and [reference/typescript-cheatsheet.md](../reference/typescript-cheatsheet.md).

---

## Type
A compile-time description of a value's shape. Erased at runtime.

## Interface
A named object type, extendable and mergeable. Preferred for object/props shapes.

## Type Alias
`type X = …` — names any type, including unions, intersections, and primitives.

## Union Type
`A | B` — a value that can be one of several types. Narrow before use.

## Intersection Type
`A & B` — a value that has all members of both types.

## Generic
A type parameter (`<T>`) that makes a function/type reusable across many types while staying type-safe.

## Narrowing
Refining a broad type to a more specific one via checks (`typeof`, `in`, discriminated unions).

## Type Guard
A boolean-returning function (`x is Foo`) that tells the compiler a value's specific type.

## Utility Type
Built-in generic helpers: `Partial<T>`, `Pick<T, K>`, `Omit<T, K>`, `Record<K, V>`, `Readonly<T>`.

## Discriminated Union
A union of object types sharing a literal "tag" field, enabling exhaustive, safe branching.

## `unknown` vs `any`
`unknown` is the type-safe top type (must be narrowed before use); `any` disables checking — avoid it.

## Literal Type
A type that is one exact value, e.g. `"success"` or `200`.

## Structural Typing
TypeScript's model: types are compatible if their shapes match, regardless of name.

## `strict` Mode
The tsconfig flag enabling all strict checks (`strictNullChecks`, `noImplicitAny`, …). Always on in this project.
