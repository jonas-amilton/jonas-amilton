# Production Incidents

Use this guide when there is real or potential impact to users, data, revenue, SLA/SLO, security, or critical operations.

## Safe sequence

1. **Classify severity**
   - What is unavailable or incorrect?
   - How many users/tenants/regions/jobs are affected?
   - Is there risk of data loss/corruption, leakage, incorrect billing, or regulatory violation?

2. **Contain damage**
   - Rollback, feature-flag disable, job pause, rate limit, temporary block, failover, or read-only mode.
   - Prefer reversible, small, observable action.
   - Do not run destructive scripts without dry-run, backup, and idempotency criteria.

3. **Preserve evidence**
   - UTC time window, request IDs, trace IDs, deploy/commit, configuration, queues, sampled payloads, and metrics.
   - Record commands and queries used. Record limits too: sampling, missing data, clock skew, retention.

4. **Diagnose by boundaries**
   - Client → gateway → API → service → database/cache/queue → external dependency.
   - At each boundary, verify input, output, latency, error, timeout, retry, and volume.

5. **Fix definitively**
   - One root cause, one primary fix.
   - Add a regression test or operational detector.
   - If an external dependency is involved, define timeout, retry/backoff, fallback, and alert.

6. **Prevent recurrence**
   - Actionable metric/alert, runbook, test, constraint, validation, dashboard, or process change.
   - Do not create an alert without owner, threshold, and expected action.

## Executive update template

```markdown
Status: investigating | mitigated | fixed | monitoring
Impact: [who/what/since when]
Evidence: [logs, metrics, traces, commits]
Mitigation: [reversible action taken]
Next step: [action + owner + time]
Residual risk: [what can still fail]
```

## Rules for operational scripts

- Idempotent by default: rerunning does not duplicate effect.
- Dry-run when writing to real data.
- Small batches with checkpoint and resume.
- Timeout and bounded retry for I/O.
- Log counters: read, changed, skipped, failed.
- Never log secrets or unnecessary PII.
