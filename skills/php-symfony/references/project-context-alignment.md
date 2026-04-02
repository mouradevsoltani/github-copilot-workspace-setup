# Project Context Alignment

## Objective
Avoid conflicts between generic Symfony guidance and project-specific conventions.

## Precedence Rule
1. Repository local instructions (e.g., `.github/instructions/*`) have highest priority.
2. This `php-symfony` skill provides defaults and fallback conventions.

## Alignment Steps
- Confirm actual Symfony/PHP versions.
- Confirm ORM mapping mode (attributes vs docblock annotations).
- Confirm serializer stack and conventions.
- Confirm base controller conventions (`AbstractController` vs custom base).
- Confirm auth stack and gateway patterns.

## Conflict Resolution
- If a local standard conflicts with this skill, follow local standard and document the exception in the implementation notes.

## Checklist
- Technical assumptions are validated before code changes.
- Stack-specific constraints are respected in generated code.
