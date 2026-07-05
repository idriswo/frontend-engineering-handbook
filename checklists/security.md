# ✅ Security Checklist

Reference: [17-authentication.md](../docs/17-authentication.md) and [glossary/security.md](../glossary/security.md).

## Secrets & Config

- [ ] No secrets, API keys, or tokens committed to the repo
- [ ] `.env` files gitignored; only `.env.example` committed
- [ ] Only `VITE_`-prefixed vars exposed to the client — nothing sensitive
- [ ] Third-party keys are public/restricted keys, not server secrets

## Authentication & Authorization

- [ ] Tokens stored appropriately (prefer httpOnly cookies over localStorage where possible)
- [ ] Protected routes enforced by guards, not just hidden UI
- [ ] Authorization checked on the **server** — client checks are UX only
- [ ] Tokens cleared on logout; refresh flow handles expiry safely

## Input & Output

- [ ] All user input validated (client + server) — see [19-zod.md](../docs/19-zod.md)
- [ ] No `dangerouslySetInnerHTML` with untrusted content (XSS)
- [ ] External links use `rel="noopener noreferrer"`
- [ ] URLs/redirects validated against an allowlist (no open redirects)

## Dependencies & Transport

- [ ] `npm audit` reviewed; no known high/critical vulnerabilities
- [ ] Dependencies pinned and kept up to date
- [ ] All API calls over HTTPS
- [ ] Security headers/CSP configured at the host/CDN

---

**Rule of thumb:** never trust the client. Validate and authorize on the server.
