# ✅ Before Merge Checklist

For reviewers and authors before merging a pull request into `main`.

## CI & Quality Gates

- [ ] All CI checks green (build, lint, type-check, tests)
- [ ] No decrease in test coverage for changed code
- [ ] No new ESLint/TypeScript warnings introduced

## Code Review

- [ ] At least one approving review from another engineer
- [ ] All review comments resolved or explicitly deferred with a ticket
- [ ] Follows handbook rules: architecture, naming, clean code, SOLID
- [ ] No copy-paste duplication (DRY) — see [05-dry-kiss-yagni.md](../docs/05-dry-kiss-yagni.md)

## Scope & Safety

- [ ] PR is focused — one logical change, not a mixed bag
- [ ] No unrelated refactors bundled in
- [ ] Breaking changes documented and communicated
- [ ] Feature flags / config in place for risky changes

## Final

- [ ] Branch up to date with `main`
- [ ] Squash-merge with a clean Conventional Commit title

---

**Rule of thumb:** merge only what you'd be comfortable shipping to production today.
