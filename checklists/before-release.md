# ✅ Before Release Checklist

Before tagging a release / deploying a new version to production.

## Verification

- [ ] All merged features tested on staging
- [ ] Full regression pass on critical user flows (auth, checkout, core CRUD)
- [ ] Cross-browser + responsive check (Chrome, Firefox, Safari, mobile)
- [ ] No known blocker/critical bugs open

## Build & Config

- [ ] Production build succeeds with production env vars
- [ ] Environment variables set correctly per environment (no staging URLs in prod)
- [ ] Source maps handled per policy (uploaded to monitoring, not exposed publicly)
- [ ] Bundle size within budget — see [performance.md](performance.md)

## Release Management

- [ ] Version bumped following SemVer
- [ ] CHANGELOG / release notes updated
- [ ] Git tag created (`vX.Y.Z`)
- [ ] Rollback plan documented (previous version + revert steps)

## Post-deploy

- [ ] Monitoring/alerts active (errors, performance)
- [ ] Smoke test on production after deploy
- [ ] Stakeholders notified

---

**Rule of thumb:** a release you can't roll back is a release you shouldn't ship.
