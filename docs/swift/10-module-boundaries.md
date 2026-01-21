# Module Boundaries & Dependencies

## Principles
- Dependencies must be acyclic
- Feature modules own feature logic; app targets only compose
- Keep public API surfaces minimal and intentional
- Prefer compile-time boundaries to enforce architecture

## Dependency direction
Allowed direction (high level):
- App targets -> Feature/UI modules
- Feature/UI modules -> Core/Infrastructure modules
- Core/Infrastructure modules -> Foundation/Apple frameworks

Not allowed:
- Feature modules depending on app targets
- Infrastructure modules depending on features or UI
- Cross-feature dependencies without explicit approval

## Public API rules
- Default to `internal`
- Mark `public` only when used across module boundaries
- Do not expose concrete types if a protocol will do
- Avoid `@_spi` unless explicitly approved

## Protocol placement
- Place protocols in the module that consumes them
- Implementations live in the providing module
- Wire via dependency injection at the composition root

## Resource ownership
- A module owns its resources; do not load resources across modules
- Use resource bundles or explicit APIs for cross-module assets

## Testing boundaries
- Unit tests live with the module they test
- Avoid test-only dependencies in production targets
- Use `@testable import` only within the module's test target

## Review checklist
- No dependency cycles
- Public API surface justified
- Module responsibilities are cohesive
- No cross-feature coupling without approval
