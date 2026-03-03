# Idempotency
- **Idempotency keys**: request_hash for intake; capsule_id for all downstream steps.
- **Deduplication strategy**: reuse cached artifacts if capsule_id already exists; prevent double audit append.
- **Side-effect boundaries**: only report-and-audit writes to durable decision ledger.
