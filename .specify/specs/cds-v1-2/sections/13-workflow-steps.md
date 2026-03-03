# Workflow steps

## Step Graph
Request Intake -> Snapshot Resolve -> Capsule Build -> Governance Check -> Simulation -> Liquidity Cascade -> Lexicographic Solve -> Report + Audit Persist

## Step Definitions

### Step 1: request-intake
- **Responsibilities**: Validate schema, normalize request, assign request_id.
- **Inputs**: Raw request payload.
- **Outputs**: Canonical request JSON.
- **Timeout**: 30s
- **Retries**:
  - Max attempts: 2
  - Backoff: fixed
  - Non-retriable errors: schema invalid, missing required fields
- **Idempotency**:
  - Idempotency key: request_hash
  - Deduplication rules: return existing decision if request_hash already processed
- **Secrets used**: none
- **Observability**:
  - Events emitted: step_started, step_completed, step_failed
  - Metrics: request_validation_failures

### Step 2: snapshot-resolve
- **Responsibilities**: Resolve portfolio and market snapshots by content address.
- **Inputs**: snapshot refs
- **Outputs**: Snapshot payloads + hashes
- **Timeout**: 60s
- **Retries**:
  - Max attempts: 3
  - Backoff: exponential
  - Non-retriable errors: snapshot not found, hash mismatch
- **Idempotency**:
  - Idempotency key: snapshot_ref
  - Deduplication rules: cache resolved snapshots by hash
- **Secrets used**: snapshot store credentials
- **Observability**:
  - Events emitted: snapshot_resolved
  - Metrics: snapshot_resolve_latency

### Step 3: capsule-build
- **Responsibilities**: Construct Decision Capsule and compute capsule_id.
- **Inputs**: Canonical request, snapshots, config version, runtime fingerprint
- **Outputs**: Capsule bundle
- **Timeout**: 60s
- **Retries**:
  - Max attempts: 2
  - Backoff: fixed
  - Non-retriable errors: signature failure, missing runtime profile
- **Idempotency**:
  - Idempotency key: capsule_id
  - Deduplication rules: no overwrite; return existing capsule
- **Secrets used**: signing keys
- **Observability**:
  - Events emitted: CAPSULE_CREATED
  - Metrics: capsule_build_seconds

### Step 4: governance-check
- **Responsibilities**: Validate config version signatures and parameter tiers.
- **Inputs**: Capsule config metadata
- **Outputs**: Approved config or halt
- **Timeout**: 30s
- **Retries**:
  - Max attempts: 1
  - Backoff: none
  - Non-retriable errors: missing signatures, tier violations
- **Idempotency**:
  - Idempotency key: config_version
  - Deduplication rules: reuse validation results by config_version
- **Secrets used**: signature verification keys
- **Observability**:
  - Events emitted: CONFIG_UPDATE
  - Metrics: config_validation_failures

### Step 5: scenario-simulate
- **Responsibilities**: Generate scenario paths based on mode, regimes, and ambiguity sets.
- **Inputs**: Capsule, market state, governed parameters
- **Outputs**: Scenario catalog or WDRO distribution
- **Timeout**: 10m
- **Retries**:
  - Max attempts: 1
  - Backoff: none
  - Non-retriable errors: determinism mismatch, data integrity failure
- **Idempotency**:
  - Idempotency key: capsule_id
  - Deduplication rules: reuse scenario cache for identical capsule_id
- **Secrets used**: none
- **Observability**:
  - Events emitted: RUN_START
  - Metrics: scenario_generation_seconds

### Step 6: liquidity-cascade
- **Responsibilities**: Run pathwise liquidity model with fixed-point loop and impact model.
- **Inputs**: Scenario paths, portfolio state, liquidity model config
- **Outputs**: Liquidity minima, margin calls, convergence status
- **Timeout**: 10m
- **Retries**:
  - Max attempts: 1
  - Backoff: none
  - Non-retriable errors: CASCADE_NONCONVERGENCE
- **Idempotency**:
  - Idempotency key: capsule_id
  - Deduplication rules: reuse computed paths when deterministic signature matches
- **Secrets used**: none
- **Observability**:
  - Events emitted: CASCADE_NONCONVERGENCE
  - Metrics: liquidity_minima, cascade_iterations

### Step 7: lexicographic-solve
- **Responsibilities**: Solve guardrail stages and optimize conditional objective.
- **Inputs**: Risk metrics, liquidity paths, constraints, decision variables
- **Outputs**: Decision action, feasibility certificate
- **Timeout**: 15m
- **Retries**:
  - Max attempts: 1
  - Backoff: none
  - Non-retriable errors: solver_infeasible, determinism mismatch
- **Idempotency**:
  - Idempotency key: capsule_id
  - Deduplication rules: reuse solver result for same capsule
- **Secrets used**: none
- **Observability**:
  - Events emitted: SOLVER_FEASIBLE, SOLVER_INFEASIBLE
  - Metrics: solver_time_seconds

### Step 8: report-and-audit
- **Responsibilities**: Assemble risk report, attach audit artifacts, persist results.
- **Inputs**: Decision, solver logs, capsule metadata
- **Outputs**: Final output payload and audit log events
- **Timeout**: 60s
- **Retries**:
  - Max attempts: 2
  - Backoff: fixed
  - Non-retriable errors: audit ledger append failure
- **Idempotency**:
  - Idempotency key: capsule_id
  - Deduplication rules: do not double-append events; use hash chain
- **Secrets used**: audit ledger credentials
- **Observability**:
  - Events emitted: decision_persisted
  - Metrics: report_generation_seconds
