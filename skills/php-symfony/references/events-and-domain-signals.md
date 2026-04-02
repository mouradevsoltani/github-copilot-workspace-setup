# Events and Domain Signals

## Objective
Use events to decouple side effects from core business logic.

## Rules
- Emit events from services when a meaningful domain state change occurs.
- Keep event payloads minimal, explicit, and serializable.
- Do not place business rules inside event listeners/subscribers.
- Ensure listeners are idempotent when replay or retry is possible.

## Event Design
- Name events in past tense (e.g., `UserRegistered`, `InvoicePaid`).
- Include stable identifiers instead of full aggregate objects.
- Add correlation metadata where useful for tracing.

## Usage Guidance
- Service performs transaction-safe business logic.
- Event dispatch follows successful state transition.
- Subscribers handle notifications, indexing, analytics, integrations.

## Checklist
- Event contracts are version-safe.
- Side effects are observable and retry-safe.
- No hidden coupling to HTTP layer.
