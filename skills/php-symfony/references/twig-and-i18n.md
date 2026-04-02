# Twig and Internationalization

## Objective
Keep presentation safe and translatable by design.

## Twig Guidelines
- Keep templates presentation-focused only.
- Use snake_case for template names/variables.
- Escape output by default.
- Use `|raw` only for trusted and sanitized content.
- Use partial naming conventions consistently (e.g., underscore-prefixed fragments).

## Internationalization
- Use translation keys, not hardcoded user-facing strings.
- Favor descriptive keys by purpose.
- Keep translation catalogs consistent and reviewed.
- Use XLIFF (or project standard format) consistently.

## Checklist
- No business logic in Twig templates.
- No raw untrusted HTML rendering.
- New features include translation keys and messages.
