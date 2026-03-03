# Security

## Identity Model
- **Service account**: cds-decision-runner
- **Roles/permissions**: read snapshots, verify signatures, write audit events, read governed configs.

## Threat Model Summary
- Input tampering mitigated by snapshot hash verification and canonical request hashing.
- Config drift mitigated by multi-sig verification and immutable versioning.
- Replay spoofing mitigated by capsule signature validation.

## Compliance
- **Control families**: Access Control, Audit Logging, Change Management.
- **Verification standards**: Internal model risk management policy, deterministic replay checks.
