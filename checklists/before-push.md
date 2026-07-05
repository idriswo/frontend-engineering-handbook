# ✅ Before Push Checklist

Run before `git push`. This is your last local gate before CI and reviewers see the code.

## Build & Verify

- [ ] Full build succeeds (`npm run build`)
- [ ] Type-check passes across the whole project (`tsc --noEmit`)
- [ ] Lint passes with zero errors (`npm run lint`)
- [ ] All tests pass (`npm test`)

## Branch

- [ ] Working on a feature branch, not `main`/`master`
- [ ] Branch is rebased/merged with latest `main` (no stale base)
- [ ] Merge conflicts resolved and re-tested
- [ ] Commit history is clean (squashed noise/fixup commits if needed)

## Review-readiness

- [ ] No secrets or `.env` values in any commit
- [ ] Large/generated files excluded via `.gitignore`
- [ ] PR description drafted: what changed, why, how to test

---

**Rule of thumb:** never push something you haven't built and run locally at least once.
