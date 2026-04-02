# Observability and Operational Signals

## Objective
Make production behavior diagnosable with logs, metrics, and traces.

## Baseline
- Structured logs with correlation/request identifiers.
- Key metrics for latency, error rate, throughput, and queue depth.
- Alerts for sustained error spikes and degraded dependencies.

## Symfony Focus
- Include route name, user context (non-sensitive), and request id in logs.
- Instrument critical services and external API calls.
- Track retry/failure counts for Messenger handlers.

## Checklist
- Every critical flow has success/failure metrics.
- Alerts map to clear runbook actions.
- Logs avoid secrets and personal data.
