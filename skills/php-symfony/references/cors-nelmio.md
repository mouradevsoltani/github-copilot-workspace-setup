# CORS Configuration with NelmioCorsBundle

## Objective
Allow legitimate cross-origin frontend access while minimizing exposure.

## Core Rules
- Deny by default, allow only known origins.
- Scope CORS by path (`^/api/`) instead of global wildcard.
- Allow only required methods and headers.
- Use credentials only when strictly necessary.

## NelmioCorsBundle Practices
- Configure `nelmio_cors` per path and environment.
- Keep production origins explicit; avoid `*` for authenticated APIs.
- Set `max_age` to reduce preflight noise while staying safe.

## Safe Defaults
- `allow_origin`: explicit list (env-driven per environment)
- `allow_methods`: `GET, POST, PUT, PATCH, DELETE, OPTIONS` as needed
- `allow_headers`: minimal required set
- `expose_headers`: only if client needs specific headers
- `allow_credentials`: false unless required

## Security Checklist
- No wildcard origin with credentials enabled.
- Sensitive endpoints are not accidentally CORS-open.
- Preflight requests return expected headers only.
- CORS policy is tested from allowed and denied origins.

## Troubleshooting
- Validate browser preflight (`OPTIONS`) behavior.
- Check proxy/load-balancer header forwarding.
- Ensure CORS config matches actual API path prefixes.
