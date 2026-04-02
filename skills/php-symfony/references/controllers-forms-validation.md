# Controllers, Forms, and Validation

## Objective
Enforce clean boundaries at input/output layers.

## Controllers
- Keep controllers thin: map HTTP, validate, call one service, return response.
- Do not place business logic in controllers.
- Use attributes/annotations according to project conventions.
- Use explicit dependencies via constructor injection.

## Forms
- Define forms in dedicated form classes.
- Do not construct forms directly in controllers for reusable flows.
- Keep submit controls in templates unless multiple submit paths require controller-level handling.

## Validation
- Validate input at boundaries (DTO/Form/Request mapping).
- Prefer reusable object-level constraints when logic is shared.
- Return consistent validation error contracts.

## Checklist
- No business logic in controller actions.
- Every write path has validation.
- Validation failures are tested and stable.
