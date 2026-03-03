# Appendices

## YAML/JSON Examples
Example event log entry:
```json
{
  "event": "SOLVER_FEASIBLE",
  "timestamp": "2026-03-03T12:00:00Z",
  "capsule_id": "sha256:...",
  "prev_hash": "sha256:...",
  "payload_hash": "sha256:..."
}
```

## Code Snippets
Minimal capsule hash computation (pseudocode):
```text
capsule_id = sha256(canonical_request + snapshot_hashes + config_version + runtime_fingerprint)
```
