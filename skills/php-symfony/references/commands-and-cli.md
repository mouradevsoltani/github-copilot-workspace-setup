# Commands and CLI Workflows

## Objective
Build maintainable CLI commands for operational and batch tasks.

## Rules
- Keep command classes thin: parse input, call one service, print outcome.
- Business logic belongs in services, not command `execute()` methods.
- Validate command arguments/options before running side effects.
- Return meaningful exit codes for automation.

## Symfony Console Practices
- Use clear command names and descriptions.
- Support `--no-interaction` for CI/automation.
- Keep output structured and concise for logs.
- Use progress indicators only for long-running interactive operations.

## Reliability
- Make command execution resumable where possible.
- Guard against duplicate processing in batch jobs.
- Log summary metrics: processed, skipped, failed.

## Checklist
- Command is idempotent or documents non-idempotency.
- Failures provide actionable diagnostics.
- Exit code semantics are tested.
