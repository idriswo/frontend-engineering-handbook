# ✅ Production Readiness Checklist

A change is "production-ready" only when all of these hold. This is the master gate.

## Functionality

- [ ] Feature works end-to-end as specified
- [ ] Loading, error, empty, and success states handled
- [ ] Edge cases covered (empty data, slow network, denied permissions)

## Quality

- [ ] Type-safe, lint-clean, formatted
- [ ] Meaningful tests (unit/integration) passing — see [26-testing.md](../docs/26-testing.md)
- [ ] No dead code, TODOs, or debug logging left

## Non-functional

- [ ] [Performance](performance.md) checklist passed
- [ ] [Security](security.md) checklist passed
- [ ] Accessible: keyboard nav, focus states, labels, contrast (WCAG AA)
- [ ] Responsive across target breakpoints

## Observability

- [ ] Errors reported to monitoring (e.g. Sentry) via error boundaries
- [ ] Key user actions/metrics tracked if required

## Ops

- [ ] Env vars documented and set per environment
- [ ] Rollback path exists
- [ ] Docs / README updated for the change

---

**Rule of thumb:** "works on my machine" is not production-ready. Verify the real deployed behavior.
