# Service Boundaries and Use-Case Design

## Objective
Keep services cohesive, testable, and aligned with use-case boundaries.

## Rules
- One service method should target one business use-case.
- Services orchestrate validation, repositories, domain policies, and events.
- Avoid cross-service cycles and hidden side effects.
- Keep transaction boundaries explicit.

## Dependency Rules
- Depend on abstractions where practical.
- Avoid leaking framework-specific objects into domain methods.
- Keep mapping concerns in dedicated mappers/assemblers when complexity grows.

## Checklist
- Method names express business intent.
- Service dependencies are minimal and explicit.
- Unit tests cover critical branches and failure paths.
