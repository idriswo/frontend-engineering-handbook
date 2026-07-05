# HTTP Status Codes

Quick reference for HTTP status codes. See [16-axios.md](../docs/16-axios.md) and [25-error-handling.md](../docs/25-error-handling.md).

## 1xx — Informational
| Code | Meaning |
| --- | --- |
| 100 | Continue |
| 101 | Switching Protocols |

## 2xx — Success
| Code | Meaning | Use |
| --- | --- | --- |
| 200 | OK | Successful GET/PUT |
| 201 | Created | Successful POST creating a resource |
| 202 | Accepted | Async processing started |
| 204 | No Content | Success with no body (DELETE) |

## 3xx — Redirection
| Code | Meaning |
| --- | --- |
| 301 | Moved Permanently |
| 302 | Found (temporary redirect) |
| 304 | Not Modified (cache hit) |
| 307 / 308 | Temporary / Permanent Redirect (method preserved) |

## 4xx — Client Errors
| Code | Meaning | Frontend handling |
| --- | --- | --- |
| 400 | Bad Request | Show validation errors |
| 401 | Unauthorized | Refresh token or redirect to login |
| 403 | Forbidden | Show "no access" / redirect to /forbidden |
| 404 | Not Found | Show empty/not-found state |
| 405 | Method Not Allowed | Bug — fix the request |
| 409 | Conflict | Show conflict message (e.g., duplicate) |
| 422 | Unprocessable Entity | Field-level validation errors |
| 429 | Too Many Requests | Back off / retry with delay |

## 5xx — Server Errors
| Code | Meaning | Frontend handling |
| --- | --- | --- |
| 500 | Internal Server Error | Show generic error + retry |
| 502 | Bad Gateway | Retry / show error |
| 503 | Service Unavailable | Retry with backoff |
| 504 | Gateway Timeout | Retry / show timeout error |

## Quick rules
- **2xx** → success path (still handle empty data).
- **401** → auth flow (interceptor refresh).
- **4xx** → user/client problem → actionable message.
- **5xx** → server problem → generic message + retry.
