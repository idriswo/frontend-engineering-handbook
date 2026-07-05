# REST API Status & Conventions

Quick reference for REST API design conventions. See [14-rtk-query.md](../docs/14-rtk-query.md) and [16-axios.md](../docs/16-axios.md).

## HTTP methods → CRUD

| Method | CRUD | Idempotent | Body | Typical success |
| --- | --- | --- | --- | --- |
| GET | Read | Yes | No | 200 |
| POST | Create | No | Yes | 201 |
| PUT | Replace | Yes | Yes | 200 / 204 |
| PATCH | Partial update | No | Yes | 200 |
| DELETE | Delete | Yes | No | 204 |

## Resource naming

```
GET    /users               # list
POST   /users               # create
GET    /users/:id           # read one
PUT    /users/:id           # replace
PATCH  /users/:id           # partial update
DELETE /users/:id           # delete
GET    /users/:id/orders    # nested collection
```

- Use **plural nouns** (`/users`, not `/getUser`).
- No verbs in paths — the HTTP method is the verb.
- Use **kebab-case** for multi-word paths (`/order-items`).

## Query parameters

```
GET /users?page=2&limit=20            # pagination
GET /users?sort=-createdAt            # sorting (- = desc)
GET /users?role=admin&isActive=true   # filtering
GET /users?search=jane                # search
GET /users?fields=id,name             # sparse fieldsets
```

## Standard response shapes

```jsonc
// Success (collection)
{ "data": [ ... ], "meta": { "page": 1, "total": 240 } }

// Success (single)
{ "data": { "id": "...", "name": "..." } }

// Error
{ "error": { "code": "VALIDATION_ERROR", "message": "Invalid email", "fields": { "email": "Invalid" } } }
```

## Status code mapping

| Action | Success | Common errors |
| --- | --- | --- |
| Create | 201 | 400, 409, 422 |
| Read | 200 | 401, 403, 404 |
| Update | 200 / 204 | 400, 404, 409, 422 |
| Delete | 204 | 401, 403, 404 |
| Auth | 200 | 401 |

## Best practices
- Version the API (`/v1/...`).
- Return consistent error shapes (normalize on the client — chapter 16).
- Use tags/ETags for caching; RTK Query mirrors this with tag invalidation.
