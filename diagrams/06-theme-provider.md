# Theme Provider

Dark/light mode with system preference and persistence. See [30-best-practices.md](../docs/30-best-practices.md).

```mermaid
graph TD
  START[App mounts] --> READ{localStorage theme?}
  READ -->|found| USE[Use stored theme]
  READ -->|none| SYS{prefers-color-scheme}
  SYS -->|dark| DARK[theme = dark]
  SYS -->|light| LIGHT[theme = light]

  USE --> APPLY[set data-theme on documentElement]
  DARK --> APPLY
  LIGHT --> APPLY
  APPLY --> TOKENS[Tailwind tokens react to data-theme]

  TOGGLE[User toggles theme] --> PERSIST[Save to localStorage]
  PERSIST --> APPLY
```

**Key idea:** theme is centralized, respects system preference on first load, persists user choice, and drives Tailwind tokens.
