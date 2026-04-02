# DTO Design and Mapping

## Objective
Use DTOs to enforce clear boundaries between transport, domain logic, and persistence.

## Core Rules
- Use dedicated input DTOs for incoming requests.
- Use dedicated output DTOs for API responses.
- Do not expose Doctrine entities directly to external contracts.
- Keep DTOs explicit, typed, and focused on one use-case.

## Validation
- Validate input DTOs at boundaries (controller/form/request mapper).
- Keep validation constraints close to DTO definitions.
- Return stable validation error shape when constraints fail.

## Mapping Strategy
- Request payload -> Input DTO
- Input DTO -> Service/use-case arguments
- Domain result/entity -> Output DTO
- Output DTO -> JSON response

## Design Guidelines
- Prefer immutable DTOs where possible.
- Use clear naming (`CreateUserInput`, `UserDetailsOutput`).
- Avoid business logic inside DTOs.
- Keep nested DTO depth reasonable for readability.

## Versioning and Compatibility
- Treat output DTO changes as API contract changes.
- Add new fields in backward-compatible way when possible.
- Document deprecated fields and removal timeline.

## Checklist
- Every write endpoint uses an input DTO.
- Every public response uses an output DTO.
- Mappings are covered by tests for critical flows.
- DTO contracts are documented in OpenAPI/Nelmio.
