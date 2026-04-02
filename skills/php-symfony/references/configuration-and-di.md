# Configuration and Dependency Injection

## Objective
Keep Symfony configuration explicit, secure, and maintainable.

## Environment Configuration
- Use environment variables only for infrastructure concerns.
- Keep behavior configuration in Symfony config files, not ad-hoc env flags.
- Store sensitive values with Symfony Secrets or external secret stores.

## Application Configuration
- Keep app parameters in `config/services.yaml` using `app.*` prefixes.
- Override by environment only when necessary.
- Prefer explicit config over hidden defaults.

## Dependency Injection Rules
- Use constructor injection by default.
- Keep services private unless exposure is required.
- Avoid service location (`$container->get()`) in application code.
- Prefer interfaces when they improve boundaries and testability.

## Checklist
- No secret committed in repo.
- No runtime behavior controlled by scattered env flags.
- Dependencies are explicit in constructors.
