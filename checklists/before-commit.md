# ✅ Before Commit Checklist

Run through this before every `git commit`. Most items are enforced automatically by lint-staged + Husky (see [29-husky.md](../docs/29-husky.md)).

## Code

- [ ] Code compiles with no TypeScript errors (`tsc --noEmit`)
- [ ] ESLint passes with no errors (`eslint .`)
- [ ] Prettier formatting applied (`prettier --write`)
- [ ] No `console.log`, `debugger`, or commented-out code left behind
- [ ] No `any` types added without justification

## Correctness

- [ ] Change does what the task asked — manually verified
- [ ] Loading, error, and empty states handled where relevant
- [ ] No hardcoded secrets, tokens, or API keys

## Hygiene

- [ ] Only related changes are staged (`git diff --staged` reviewed)
- [ ] No unintended files committed (build output, `.env`, editor files)
- [ ] Commit message follows Conventional Commits (`feat:`, `fix:`, `chore:` …)

## Tests

- [ ] New/changed logic has tests, and they pass locally (`npm test`)

---

**Rule of thumb:** if you can't describe the change in one commit message line, split it into multiple commits.
