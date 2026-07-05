# Architecture Glossary

Architectural terms used across the handbook. See [06-project-architecture.md](../docs/06-project-architecture.md).

---

## Feature-Based Architecture
Organizing code by business feature (`features/users`, `features/auth`) rather than by technical type. Each feature is self-contained and deletable.

## Separation of Concerns
Keeping distinct responsibilities (UI, state, data-fetching, business logic) in distinct modules.

## Clean Architecture
Layering the app so dependencies point inward (UI → domain), keeping core logic independent of frameworks and I/O.

## Presentational vs Container
Presentational components render UI from props; container components/hooks own data and behavior.

## Public API (Barrel)
A feature's `index.ts` that exposes only what other modules may use, hiding internals.

## Coupling
The degree of interdependence between modules. Aim for **loose coupling**.

## Cohesion
How closely the responsibilities within a module belong together. Aim for **high cohesion**.

## Single Source of Truth
Each piece of state has exactly one authoritative owner (e.g. RTK Query cache for server data).

## SOLID
Five OOP design principles applied here to components/modules. See [04-solid-principles.md](../docs/04-solid-principles.md).

## DRY / KISS / YAGNI
Don't Repeat Yourself, Keep It Simple, You Aren't Gonna Need It. See [05-dry-kiss-yagni.md](../docs/05-dry-kiss-yagni.md).

## State Management
The strategy for storing and updating app state: local state, context, and Redux Toolkit for global/server state.

## Composition
Building complex UI/behavior by combining small pieces, preferred over inheritance.

## Dependency Injection
Passing dependencies (services, clients) into a module instead of hardcoding them — improves testability.
