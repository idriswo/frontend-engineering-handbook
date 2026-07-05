# Glossary — Terms

Core terminology for the Frontend Engineering Handbook. Each term includes a definition, why it matters, and how it relates to this project.

---

## Hydration

**Definition:** The process of taking static, server-rendered HTML and attaching React's event listeners and state so it becomes interactive in the browser.

**Why it matters:** Hydration is the bridge between fast first paint (server HTML) and full interactivity (client JS). Slow or mismatched hydration causes flicker, "dead" buttons, and console warnings.

**In this project:** We ship a client-side SPA (Vite), so hydration is not part of the default flow. The term matters when integrating SSR/SSG frameworks later. See [09-react.md](../docs/09-react.md).

---

## Memoization

**Definition:** Caching the result of an expensive computation so that repeated calls with the same inputs return the stored value instead of recomputing.

**Why it matters:** It avoids redundant work and unnecessary re-renders, improving performance — but only when applied to real hotspots.

**In this project:** `useMemo` caches values, `useCallback` caches functions, and `React.memo` skips re-renders when props are referentially stable. Apply only after profiling. See [24-performance.md](../docs/24-performance.md).

---

## Re-render

**Definition:** React re-executing a component function to produce new UI after its state or props change.

**Why it matters:** Re-renders are normal and cheap, but unnecessary ones (from unstable props or misplaced state) cause jank in large trees.

**In this project:** We prefer derived values over synced state, keep state local, and stabilize props before memoizing children. See [09-react.md](../docs/09-react.md) and [24-performance.md](../docs/24-performance.md).

---

## CSR (Client-Side Rendering)

**Definition:** The browser downloads a minimal HTML shell plus JavaScript, and React renders the UI entirely on the client.

**Why it matters:** CSR gives rich interactivity and simple hosting, at the cost of a slower first meaningful paint and weaker SEO without extra work.

**In this project:** Our Vite + React SPA is CSR by default. We mitigate its costs with code splitting and lazy loading. See [11-vite.md](../docs/11-vite.md).

---

## SSR (Server-Side Rendering)

**Definition:** The server renders the React component tree to HTML on each request and sends fully-formed markup to the browser, which then hydrates it.

**Why it matters:** SSR improves first paint and SEO for content-heavy apps, at the cost of server complexity and hydration overhead.

**In this project:** Not used by default (we are a CSR SPA). Documented here as the contrast to CSR and a future option. See [24-performance.md](../docs/24-performance.md).

---

## SPA (Single-Page Application)

**Definition:** An app that loads a single HTML document and updates the view via client-side routing instead of full page reloads.

**Why it matters:** SPAs give fluid, app-like navigation and shared client state, but require attention to code splitting, routing, and initial bundle size.

**In this project:** We are an SPA using React Router with lazy-loaded routes. See [12-react-router.md](../docs/12-react-router.md).

---

## Lazy Loading

**Definition:** Deferring the loading of code or resources until the moment they are actually needed.

**Why it matters:** It shrinks the initial bundle and speeds up first load — the user only downloads what the current view requires.

**In this project:** Routes and heavy components are loaded with `React.lazy` + `<Suspense>`. See [12-react-router.md](../docs/12-react-router.md) and [24-performance.md](../docs/24-performance.md).

---

## Tree Shaking

**Definition:** A build-time optimization that removes unused (dead) exports from the final bundle by analyzing ES module imports.

**Why it matters:** It keeps bundles small by shipping only the code that is actually imported and used.

**In this project:** Enabled by ES modules + Vite/Rollup. We rely on it by using named imports (e.g., Lucide icons) rather than whole-library imports. See [23-lucide-react.md](../docs/23-lucide-react.md).

---

## Bundle Splitting

**Definition:** Breaking the compiled output into multiple smaller chunks (by route, vendor, or feature) that load independently.

**Why it matters:** It enables lazy loading and better caching — users download only the chunks they need, and unchanged vendor chunks stay cached across deploys.

**In this project:** Vite splits lazy routes automatically; vendor code is separated. See [11-vite.md](../docs/11-vite.md) and [24-performance.md](../docs/24-performance.md).

---

## Closure

**Definition:** A function that retains access to variables from the scope in which it was created, even after that scope has finished executing.

**Why it matters:** Closures power hooks, event handlers, and callbacks. Misunderstanding them causes "stale closure" bugs where a callback reads outdated state.

**In this project:** Relevant to `useEffect`/`useCallback` dependency arrays — stale closures are a common cause of subtle bugs. See [09-react.md](../docs/09-react.md).

---

## Hoisting

**Definition:** JavaScript's behavior of moving `var` and function *declarations* to the top of their scope before execution (`let`/`const` are hoisted but not initialized — the "temporal dead zone").

**Why it matters:** It explains why some references work before their apparent definition and why others throw. Understanding it prevents subtle initialization bugs.

**In this project:** We use `const`/`let` and function expressions, avoiding `var`, which sidesteps most hoisting surprises. See [03-clean-code.md](../docs/03-clean-code.md).
