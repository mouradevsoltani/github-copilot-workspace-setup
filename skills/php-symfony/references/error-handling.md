# Error Handling and API Error Contract

## Objective
Return consistent errors and avoid leaking internals.

## Rules
- Convert domain/validation exceptions to explicit HTTP responses.
- Keep error payload shape stable across endpoints.
- Log technical details server-side only.
- Never expose stack traces in production responses.

## Suggested Error Shape
```json
{
  "code": "validation_error",
  "message": "Input validation failed",
  "details": []
}
```

## Implementation Notes
- Centralize exception-to-response mapping.
- Distinguish client errors (4xx) from server errors (5xx).
- Add tests for key error scenarios and unauthorized access.
