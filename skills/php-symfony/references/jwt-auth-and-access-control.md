# JWT Authentication and Access Control

## Objective
Enforce robust API authentication and authorization with minimal attack surface.

## Rules
- Validate token signature, issuer, audience, and expiration.
- Reject weak or malformed tokens early.
- Use least-privilege role and scope checks.
- Keep authorization decisions centralized (voters/policies).

## Hardening
- Short token lifetime and refresh strategy.
- Key rotation with overlap window.
- Revoke tokens on compromise-sensitive events.

## Checklist
- Auth failures return consistent 401/403 contract.
- Scope/role checks are tested on critical endpoints.
- Rotation and revocation process is documented.
