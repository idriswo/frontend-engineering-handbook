# Security Glossary

Frontend security terms. See [17-authentication.md](../docs/17-authentication.md) and [checklists/security.md](../checklists/security.md).

---

## Authentication (AuthN)
Verifying **who** a user is (login).

## Authorization (AuthZ)
Verifying **what** a user is allowed to do (roles, permissions). Must be enforced server-side.

## JWT (JSON Web Token)
A signed, self-contained token carrying claims (user id, roles, expiry). Used for stateless auth.

## Access Token / Refresh Token
Short-lived access token authorizes requests; long-lived refresh token obtains new access tokens without re-login.

## XSS (Cross-Site Scripting)
Injecting malicious scripts into a page. Prevented by escaping output and avoiding `dangerouslySetInnerHTML` with untrusted data.

## CSRF (Cross-Site Request Forgery)
Tricking a logged-in user's browser into sending unwanted requests. Mitigated with tokens/SameSite cookies.

## CORS (Cross-Origin Resource Sharing)
Browser policy controlling which origins may call an API. Configured on the server.

## Same-Origin Policy
Browser rule restricting how documents/scripts from one origin interact with another.

## httpOnly Cookie
A cookie inaccessible to JavaScript, reducing token theft via XSS — preferred for storing tokens.

## Content Security Policy (CSP)
A header restricting which sources of scripts/styles/images may load, mitigating XSS.

## Open Redirect
A flaw where an app redirects to a user-controlled URL. Validate redirect targets against an allowlist.

## Secrets Management
Keeping keys/tokens out of the client bundle and repo. Only `VITE_`-prefixed, non-sensitive vars reach the client.

## Input Validation
Checking/sanitizing all user input on client **and** server. See [19-zod.md](../docs/19-zod.md).
