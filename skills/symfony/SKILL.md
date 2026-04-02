---
name: symfony
description: "Symfony development technical standards for this project."

---

# Symfony Skill (Technical Only)

This file defines the technical rules, conventions, architecture, and best practices for writing Symfony code in this project.

---

# Global Technical Standards
- Use PHP 8.2+ with `declare(strict_types=1)`.
- Use Symfony’s default directory structure, no custom bundles.
- Use autowiring + autoconfiguration.
- Use docblock annotations for Doctrine/JMS (no PHP 8 attributes).
- All business logic implemented in Services.
- Keep code explicit, predictable, and readable.

---

# Project Architecture (Technical)
src/
  Controller/{Domain}/
  Service/{Domain}/
  Repository/
  Entity/
  Model/{Domain}/
  Exception/
  Traits/
  Utils/
---

# Controllers (Technical responsibilities)
- Use `#[Route]` and security attributes or annotations.
- Extend `AppController`.
- Only handle HTTP mapping, input validation, calling ONE service, returning Response.
- No DB access.
- No domain logic.

---

# Services (Technical responsibilities)
- Contain domain/business logic.
- Use constructor promotion (`private readonly`).
- Should be `final`.
- Orchestrate validation, transformations, repository calls, event dispatchers.
- Use Monolog for structured logging.

---

# Repositories
- Doctrine ORM only.
- Explicit, readable query methods.
- Use parameterized queries.
- Avoid N+1 via join fetch.
- Paginate large results.

---

# Entities
- Docblock ORM mappings.
- Typed properties.
- Include Timestampable/Blameable traits.
- No business logic.

---

# Models (External Service Proxies)
- Wrap HTTP/API calls.
- Convert external data into internal DTOs.
- No domain logic inside them.

---

# Exceptions
- Domain-specific exceptions.
- Clear messages, never include sensitive data.
- Handled by controller/subscriber layers.

---

# Traits
- Allowed: TimestampableTrait, BlameableTrait.
- Must be used sparsely.

---

# Utils
- Pure helpers (no domain logic).
- FileManager, PathSecurityValidator, etc.

---

# Configuration (Technical)
- `.env` for environment-specific values only.
- Application settings in `services.yaml`, prefixed with `app.*`.
- Sensitive data stored in Symfony Secrets.

---

# Validation
- Symfony Validator is mandatory for all entry points.
- Data validated at boundaries (DTOs, forms, request handlers).

---

# Security
- Prefer one firewall.
- Authorization handled by voters.
- Avoid complex security expressions.
- Always sanitize input/output.

---

# Testing (Technical)
- Functional tests with `WebTestCase`.
- API tests through HttpClient.
- Unit tests for services.
- Smoke tests for public routes.