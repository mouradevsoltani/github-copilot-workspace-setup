# Symfony Architecture Reference

## Layering Rules
- Controllers handle HTTP mapping only and call one service.
- Services contain business logic and orchestration.
- Repositories contain persistence logic only.
- Entities contain data and ORM mapping, no business logic.
- Models wrap external API clients and adapters.
- Exceptions model domain/technical failures without leaking sensitive details.
- Util and Trait stay framework-agnostic and side-effect minimal.

## Expected Project Structure
- `src/Controller/{Domain}/`
- `src/Command/{Domain}/`
- `src/Dto/{Domain}/`
- `src/Service/{Domain}/`
- `src/Repository/`
- `src/Entity/`
- `src/Model/{Domain}/`
- `src/Event/`
- `src/Exception/`
- `src/Trait/`
- `src/Util/`

## Coding Standards
- PHP 8.2+ with `declare(strict_types=1)`.
- `final` services and constructor injection with `private readonly`.
- Explicit code over magic behavior.
- Use Symfony autowiring/autoconfiguration.
- Keep doctrine queries parameterized and readable.

## Configuration Rules
- Keep environment-specific values in `.env*` only.
- Keep application settings in `services.yaml` under `app.*` keys.
- Store secrets with Symfony Secrets or environment-level secret stores.
