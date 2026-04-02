# Messenger Reliability Patterns

## Objective
Ensure async processing is resilient, observable, and recoverable.

## Core Practices
- Keep handlers idempotent.
- Configure retry strategy per transport.
- Use failure transport for poisoned messages.
- Include correlation identifiers in message context.

## Delivery Safety
- Validate message payloads before handling.
- Avoid side effects before all preconditions pass.
- Use deduplication keys where applicable.

## Operations
- Monitor queue depth and retry rates.
- Document replay procedure from failure transport.
- Alert on repeated handler failures.
