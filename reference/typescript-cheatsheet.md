# TypeScript Cheat Sheet

Quick reference for TypeScript. See [10-typescript.md](../docs/10-typescript.md).

## Basic types

```ts
let s: string; let n: number; let b: boolean;
let arr: number[]; let tuple: [string, number];
let u: string | number;                 // union
let anything: unknown;                  // prefer over any
```

## Objects, interfaces, types

```ts
interface User { id: string; name: string; age?: number; readonly role: string; }
type Point = { x: number; y: number };
type ID = string | number;              // unions/aliases → prefer `type`
```

## Functions

```ts
const add = (a: number, b: number): number => a + b;
function greet(name: string, greeting = "Hi"): string { return `${greeting} ${name}`; }
type Handler = (e: Event) => void;
```

## Generics

```ts
function first<T>(arr: T[]): T | undefined { return arr[0]; }
interface ApiResponse<T> { data: T; status: number; }
```

## Utility types

| Utility | Meaning |
| --- | --- |
| `Partial<T>` | All props optional |
| `Required<T>` | All props required |
| `Readonly<T>` | All props readonly |
| `Pick<T, K>` | Subset of props |
| `Omit<T, K>` | All props except K |
| `Record<K, V>` | Object with keys K, values V |
| `ReturnType<F>` | Return type of function |
| `Parameters<F>` | Tuple of param types |
| `Awaited<T>` | Unwrapped promise type |

## Narrowing & guards

```ts
if (typeof x === "string") { /* x: string */ }
if ("id" in obj) { /* has id */ }
function isUser(v: unknown): v is User { return !!v && typeof v === "object" && "id" in v; }
```

## Discriminated unions

```ts
type State =
  | { status: "loading" }
  | { status: "success"; data: User }
  | { status: "error"; error: string };
```

## Keyof, typeof, as const

```ts
type Keys = keyof User;                  // "id" | "name" | ...
const roles = ["admin", "user"] as const;
type Role = (typeof roles)[number];      // "admin" | "user"
```

## Avoid

- `any` (use `unknown` + narrowing) · `as` casts (use guards) · `!` non-null unless proven.
