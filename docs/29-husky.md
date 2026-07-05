# Husky

## Purpose

This chapter defines how we use **Husky** and **lint-staged** to run automated quality gates via Git hooks: linting, formatting, type-checking, and tests before code enters the repository.

## Why It Matters

Standards that rely on human memory eventually slip. Git hooks make quality checks automatic and unavoidable — bad code is stopped at commit/push time, not discovered later in CI or production. This keeps `main` always green and shifts feedback to the earliest, cheapest moment.

## Core Concepts

**Git hooks via Husky.** Husky wires scripts into `pre-commit`, `commit-msg`, and `pre-push`.

**lint-staged.** Runs linters/formatters only on staged files for speed.

**Fast local gate, thorough CI gate.** Hooks catch the obvious fast; CI runs the full suite.

**Commit message linting.** Optional `commitlint` enforces conventional commits.

## Rules

1. `pre-commit` runs lint-staged (ESLint + Prettier) on staged files.
2. `pre-push` runs type-check and tests.
3. Hooks must be fast enough to not disrupt flow; heavy checks go to CI.
4. Do not bypass hooks (`--no-verify`) without an explicit, justified reason.
5. Keep hook scripts in version control so every contributor gets them.

## Best Practices

- Scope lint-staged to changed files to keep commits fast.
- Fail fast with clear error output.
- Mirror hook checks in CI as the authoritative gate.

## Bad Examples

```bash
# ❌ Bypassing the gate routinely.
git commit -m "wip" --no-verify
```

## Good Examples

```jsonc
// package.json
{
  "lint-staged": {
    "*.{ts,tsx}": ["eslint --fix", "prettier --write"],
    "*.{json,md,css}": ["prettier --write"]
  }
}
```

```bash
# .husky/pre-commit
npx lint-staged
```

```bash
# .husky/pre-push
npm run type-check && npm run test
```

**Real World Example:** A staged file with a lint error is auto-fixed or the commit is blocked — before it ever reaches a teammate.

**Production Example:** `pre-push` type-check catches a broken type that would have failed CI ten minutes later.

**Explanation:** Automated hooks make quality the path of least resistance and keep `main` green.

## Common Mistakes

- Routinely bypassing hooks with `--no-verify`.
- Running the full test suite on every commit (too slow).
- Hooks not committed, so only some developers have them.
- Duplicating heavy checks locally instead of deferring to CI.

## AI Instructions

- Assume pre-commit/pre-push gates run; produce code that passes lint, format, type-check, and tests.
- Never suggest bypassing hooks casually.
- Keep hook-run checks aligned with CI.

## Checklist

- [ ] Does `pre-commit` run lint-staged?
- [ ] Does `pre-push` run type-check and tests?
- [ ] Are hooks committed to the repo?
- [ ] Are hooks fast, with heavy checks in CI?

## Summary

Husky + lint-staged make quality gates automatic: lint and format on commit, type-check and test on push. Hooks keep `main` green by stopping problems at the earliest moment, with CI as the authoritative backstop.
