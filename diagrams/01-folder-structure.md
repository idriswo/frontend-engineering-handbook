# Folder Structure

The canonical feature-based structure of the application. See [07-folder-structure.md](../docs/07-folder-structure.md).

```mermaid
graph TD
  SRC[src/]
  SRC --> APP[app/]
  SRC --> FEATURES[features/]
  SRC --> SHARED[shared/]
  SRC --> MAIN[main.tsx]
  SRC --> APPTSX[App.tsx]

  APP --> STORE[store.ts]
  APP --> ROUTER[router.tsx]
  APP --> PROVIDERS[providers.tsx]

  FEATURES --> AUTH[auth/]
  FEATURES --> USERS[users/]

  AUTH --> ACOMP[components/]
  AUTH --> AHOOKS[hooks/]
  AUTH --> AAPI[authApi.ts]
  AUTH --> ATYPES[auth.types.ts]
  AUTH --> AINDEX[index.ts]

  SHARED --> UI[components/ui/]
  SHARED --> SHOOKS[hooks/]
  SHARED --> LIB[lib/]
  SHARED --> CONST[constants/]
```

**Key idea:** every feature is self-contained and mirrors the same layout, exposing a public `index.ts`.
