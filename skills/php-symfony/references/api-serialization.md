# API Serialization and DTO Boundaries

## Objective
Keep API contracts stable and avoid exposing internal entities directly.

## Rules
- Use input and output DTOs at controller boundaries.
- Do not serialize Doctrine entities directly for public APIs.
- Version contracts when introducing breaking changes.
- Keep serializer groups explicit and documented.

## Recommended Flow
1. Request payload -> Input DTO
2. Validation -> Service call
3. Domain result -> Output DTO
4. Output DTO -> JSON response

## Guardrails
- Avoid returning lazy-loaded associations directly.
- Keep API field names stable and predictable.
- Normalize dates/timezones explicitly.
