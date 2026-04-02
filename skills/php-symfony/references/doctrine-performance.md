# Doctrine Query and Performance Guidelines

## Objective
Prevent common persistence bottlenecks and keep query behavior predictable.

## Rules
- Use repository methods with explicit intent.
- Parameterize all queries.
- Paginate large collections.
- Prevent N+1 by eager loading where required.
- Add indexes for high-frequency filters.

## Query Design
- Prefer QueryBuilder for dynamic filters.
- Select only required fields for list endpoints.
- Keep writes in services, reads in repositories.

## Diagnostics
- Use Symfony profiler in dev.
- Track slow queries and high cardinality filters.
- Add regression tests for critical query paths.
