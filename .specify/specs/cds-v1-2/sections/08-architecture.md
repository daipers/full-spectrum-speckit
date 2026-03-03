# Architecture
- **Components**: capsule service, config service, simulation engine, liquidity cascade engine, risk engine, lexicographic solver orchestrator, audit log service, replay harness.
- **Deployment model**: Containerized services running in a controlled compute cluster with pinned deterministic runtimes.
- **Trust boundaries**: Capsule signing and config approvals are enforced via governance multi-sig. Simulation and solver services run under least-privilege identities.
- **State**: Immutable snapshots and capsules are content-addressed; audit logs are append-only hash-chained.
