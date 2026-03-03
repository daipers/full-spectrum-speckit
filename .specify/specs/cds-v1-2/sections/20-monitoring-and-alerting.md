# Monitoring and alerting

## Alerts
| Alert | Condition | Severity | Paging |
|-------|-----------|----------|--------|
| Determinism mismatch | determinism_mismatch_total > 0 | critical | true |
| Tier 1 infeasible spike | > 5% of runs in 1h | critical | true |
| Step timeout | step exceeds 2x timeout | warning | false |

- **Runbook**: cds-oncall-runbook
- **Ownership**: Risk Platform On-Call
