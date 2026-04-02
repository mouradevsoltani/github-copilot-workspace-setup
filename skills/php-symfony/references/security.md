# Symfony Security Reference

## Core Security Requirements
- Enforce authentication at route and controller boundaries.
- Enforce authorization with voters and explicit checks.
- Validate all request DTOs/forms at trust boundaries.
- Never trust client-derived fields for privileged decisions.
- Keep sensitive values out of code and logs.

## Session and Cookie Hardening
- Use secure cookie flags and `HttpOnly`.
- Set strict `SameSite` where possible.
- Regenerate session IDs on login and privilege changes.

## Input and Output Safety
- Use Symfony Validator constraints for all external inputs.
- Use Twig auto-escaping by default.
- Avoid `|raw` unless content is trusted and sanitized.
- Use parameterized queries via Doctrine QueryBuilder.

## CSRF and Stateful Forms
- Keep CSRF protection enabled for browser state-changing requests.
- Validate CSRF tokens in forms and sensitive actions.

## File Upload Controls
- Validate MIME type, extension, and size.
- Generate server-side filenames.
- Store uploads outside web root when possible.

## Rate Limiting and Abuse Protection
- Apply rate limiting to login, password reset, and write-heavy endpoints.
- Prefer composite limiter keys (`identity + IP`) when applicable.

## Production Hardening
- `APP_ENV=prod`, `APP_DEBUG=false` in production.
- Configure trusted proxies correctly.
- Use HTTPS and secure headers at ingress/reverse proxy.
