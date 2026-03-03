# Observability

## Logs
- **Format**: structured JSON.
- **Correlation IDs**: run_id, capsule_id, request_hash.
- **What to log**: key decisions, solver status, guardrail breaches, determinism checks.

## Metrics (SLIs)
| Metric | Type | Labels |
|--------|------|--------|
| cds_run_duration_seconds | histogram | status, mode |
| cds_success_ratio | gauge | mode |
| solver_time_seconds | histogram | stage |
| determinism_mismatch_total | counter | reason |

## Traces
- **Trace ID**: capsule_id.
- **Span hierarchy**: intake -> capsule -> simulate -> cascade -> solve -> report.
