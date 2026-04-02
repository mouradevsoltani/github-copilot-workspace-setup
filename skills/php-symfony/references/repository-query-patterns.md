# Repository Query Patterns

## Objective
Standardize repository methods for predictable query behavior.

## Rules
- Use intention-revealing method names (`findActiveByOwner`, `existsByEmail`).
- Prefer dedicated read methods over generic query leakage in services.
- Keep write orchestration in services; repositories focus on persistence operations.
- Always parameterize filters and bound values.

## Read Patterns
- Use pagination for list endpoints.
- Provide lightweight projection methods for summary views.
- Keep sorting deterministic for stable pagination.

## Checklist
- Query methods are explicit and covered by integration tests.
- No business policy encoded in repository filters without documentation.
- N+1 risks are addressed for relation-heavy endpoints.
