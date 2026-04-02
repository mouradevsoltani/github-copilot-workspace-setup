---
name: php-symfony
description: "Unified Symfony skill for architecture, implementation, security, and testing standards with references."

---

# PHP Symfony Unified Skill

This skill is the **technical hub** for Symfony work.
It defines the default technical direction and points to focused references.
Behavioral rules (decision flow, escalation, output format) are in `instructions/symfony.instructions.md`.

> Project-level instructions (`.github/instructions/`) override these defaults when they exist.

## When to use
- Designing or changing Symfony backend code.
- Reviewing technical direction for architecture, security, testing, performance, and operations.

## Core principles
- Keep strict separation of concerns across Controller, Command, Dto, Service, Repository, Entity, Model, Event, Trait, and Util layers.
- Favor explicit, testable code over magic behavior.
- Enforce secure-by-default patterns at boundaries.
- Preserve contract stability (API and error payloads).
- Treat observability and operability as first-class requirements.

## Scope and precedence
- This file is intentionally concise; implementation details live in `references/`.
- If a project has stricter local standards, local standards win.

## References
- Architecture: `skills/php-symfony/references/architecture.md`
- Configuration and DI: `skills/php-symfony/references/configuration-and-di.md`
- Controllers, forms, and validation: `skills/php-symfony/references/controllers-forms-validation.md`
- Commands and CLI workflows: `skills/php-symfony/references/commands-and-cli.md`
- Events and domain signals: `skills/php-symfony/references/events-and-domain-signals.md`
- Service boundaries and use-case design: `skills/php-symfony/references/service-boundaries.md`
- Repository query patterns: `skills/php-symfony/references/repository-query-patterns.md`
- Twig and i18n: `skills/php-symfony/references/twig-and-i18n.md`
- Project context alignment: `skills/php-symfony/references/project-context-alignment.md`
- REST OpenAPI (NelmioApiDocBundle): `skills/php-symfony/references/openapi-nelmio.md`
- CORS (NelmioCorsBundle): `skills/php-symfony/references/cors-nelmio.md`
- Security: `skills/php-symfony/references/security.md`
- Testing: `skills/php-symfony/references/testing.md`
- Logging and retention: `skills/php-symfony/references/logging-rotation.md`
- API serialization and DTO boundaries: `skills/php-symfony/references/api-serialization.md`
- DTO design and mapping: `skills/php-symfony/references/dto-design-and-mapping.md`
- Doctrine query performance: `skills/php-symfony/references/doctrine-performance.md`
- Messenger reliability: `skills/php-symfony/references/messenger-reliability.md`
- Error handling contract: `skills/php-symfony/references/error-handling.md`
- Cache and HTTP performance: `skills/php-symfony/references/cache-and-http-performance.md`
- Migrations and release safety: `skills/php-symfony/references/migrations-and-release.md`
- Observability and signals: `skills/php-symfony/references/observability.md`
- JWT auth and access control: `skills/php-symfony/references/jwt-auth-and-access-control.md`