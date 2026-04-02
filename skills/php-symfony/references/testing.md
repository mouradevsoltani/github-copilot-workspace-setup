# Symfony Testing Reference

## Test Pyramid Guidance
- Unit tests: service/domain logic and edge cases.
- Functional tests: controllers and HTTP behavior with `WebTestCase`.
- API tests: request/response contracts and auth paths.
- Integration tests: repository and infrastructure interactions when needed.

## Minimum Coverage Expectations
- Critical business flows are covered.
- Failure paths and validation errors are covered.
- Authorization-denied scenarios are covered.
- Security-sensitive flows include abuse/limit checks where relevant.

## Test Quality Rules
- Prefer behavior-focused assertions over implementation details.
- Keep tests deterministic and isolated.
- Avoid brittle global state and hidden time dependencies.
- Use clear arrange/act/assert structure.
