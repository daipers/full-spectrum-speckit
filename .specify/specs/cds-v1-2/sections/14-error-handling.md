# Error handling

## Failure Taxonomy
- **Retriable errors**: transient snapshot store timeout, audit ledger retryable failure.
- **Non-retriable errors**: schema invalid, signature invalid, determinism mismatch, CASCADE_NONCONVERGENCE.
- **Human-notification triggers**: determinism mismatch, override requested, repeated infeasibility in Tier 1-3.

## Retry Policy Summary
| Step | Max Attempts | Backoff | Timeout |
|------|-------------|---------|---------|
| request-intake | 2 | fixed | 30s |
| snapshot-resolve | 3 | exponential | 60s |
| capsule-build | 2 | fixed | 60s |
| governance-check | 1 | none | 30s |
| scenario-simulate | 1 | none | 10m |
| liquidity-cascade | 1 | none | 10m |
| lexicographic-solve | 1 | none | 15m |
| report-and-audit | 2 | fixed | 60s |
