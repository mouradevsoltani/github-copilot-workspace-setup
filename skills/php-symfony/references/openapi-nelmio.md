# REST API Documentation with OpenAPI (NelmioApiDocBundle)

## Objective
Expose clear, versioned, and testable API contracts using OpenAPI generated from Symfony endpoints.

## Core Rules
- Document every public REST endpoint.
- Keep request/response schemas aligned with DTOs.
- Version API contracts when introducing breaking changes.
- Ensure security schemes are declared (Bearer/JWT, API key, etc.).

## Setup Baseline
- Install and configure `nelmio/api-doc-bundle`.
- Keep documentation routes protected in non-dev environments.
- Organize docs by areas/tags (public, admin, internal).

## Symfony Practices
- Use explicit route names and HTTP methods.
- Annotate/attribute operations, parameters, and response models.
- Reuse schema components to avoid duplication.
- Keep examples realistic and non-sensitive.

## Contract Quality Checklist
- All required fields are marked as required.
- Error responses (4xx/5xx) are documented with consistent shape.
- Authentication requirements are explicit per endpoint.
- Pagination/filter/sort query params are documented.

## CI/Review Checklist
- API docs build without warnings.
- OpenAPI diff is reviewed on contract changes.
- Consumer-facing breaking changes are highlighted in release notes.
