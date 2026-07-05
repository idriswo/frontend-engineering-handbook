# Regex Cheat Sheet

Quick reference for regular expressions (JavaScript flavor).

## Character classes

| Pattern | Matches |
| --- | --- |
| `.` | Any char except newline |
| `\d` / `\D` | Digit / non-digit |
| `\w` / `\W` | Word char `[A-Za-z0-9_]` / non-word |
| `\s` / `\S` | Whitespace / non-whitespace |
| `[abc]` | a, b, or c |
| `[^abc]` | Not a, b, or c |
| `[a-z]` | Range |

## Anchors & boundaries

| Pattern | Meaning |
| --- | --- |
| `^` | Start of string/line |
| `$` | End of string/line |
| `\b` / `\B` | Word boundary / non-boundary |

## Quantifiers

| Pattern | Meaning |
| --- | --- |
| `*` | 0 or more |
| `+` | 1 or more |
| `?` | 0 or 1 (optional) |
| `{n}` | Exactly n |
| `{n,}` | n or more |
| `{n,m}` | Between n and m |
| `*?` `+?` | Lazy (non-greedy) |

## Groups & alternation

| Pattern | Meaning |
| --- | --- |
| `(abc)` | Capturing group |
| `(?:abc)` | Non-capturing group |
| `(?<name>abc)` | Named group |
| `a\|b` | a or b |
| `(?=abc)` | Positive lookahead |
| `(?!abc)` | Negative lookahead |
| `(?<=abc)` | Positive lookbehind |

## Flags

`g` global · `i` case-insensitive · `m` multiline · `s` dotall · `u` unicode · `y` sticky

## JS usage

```ts
const re = /^\w+@\w+\.\w+$/i;
re.test("a@b.co");                       // boolean
"a1b2".match(/\d/g);                     // ["1", "2"]
"a1b2".replace(/\d/g, "#");              // "a#b#"
"2026-07-05".match(/(?<y>\d{4})-(?<m>\d{2})/)?.groups?.y;  // "2026"
```

## Common patterns

```ts
const email = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
const url = /^https?:\/\/[^\s]+$/;
const uuid = /^[0-9a-f]{8}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{12}$/i;
const slug = /^[a-z0-9]+(?:-[a-z0-9]+)*$/;
```

> Prefer Zod validators over hand-rolled regex for user-facing validation. See [19-zod.md](../docs/19-zod.md).
