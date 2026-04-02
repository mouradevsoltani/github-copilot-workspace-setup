# Migrations and Release Safety

## Objective
Ship schema changes safely and reversibly.

## Rules
- Every schema change uses a migration.
- Migrations are small, reviewable, and ordered.
- Avoid destructive operations without rollback planning.
- Validate migration runtime on realistic data volume.

## Release Pattern
1. Deploy backward-compatible code.
2. Run additive migrations.
3. Enable new behavior.
4. Remove legacy fields in a later release.

## Checklist
- Migration has up/down or an explicit rollback plan.
- Long-running operations are batched when needed.
- Release runbook includes migration step and verification.
