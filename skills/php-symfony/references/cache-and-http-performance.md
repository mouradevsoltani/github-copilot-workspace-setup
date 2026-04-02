# Cache and HTTP Performance

## Objective
Improve response times while preserving correctness and cache safety.

## Rules
- Use HTTP cache headers intentionally (`Cache-Control`, `ETag`, `Last-Modified`).
- Cache only idempotent read paths.
- Separate private and public cache behavior.
- Invalidate cache on write operations affecting cached resources.

## Symfony Guidance
- Prefer explicit cache policies in controllers.
- Use cache pools for application-level computed data.
- Avoid caching user-specific sensitive payloads in shared caches.

## Checklist
- Read endpoints define cache strategy.
- Write endpoints trigger invalidation strategy.
- Cache hit ratio and stale behavior are monitored.
