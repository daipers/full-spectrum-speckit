# Non-functional requirements
- Performance: p95 decision time <= 20 minutes under nominal load.
- Security: configs and capsules MUST be signed and verified; secrets never logged.
- Reliability: determinism mismatch rate must be 0 in production replays.
