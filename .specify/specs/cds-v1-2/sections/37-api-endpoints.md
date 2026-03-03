# API endpoints
- **POST /cds/decisions**: Submit a decision request; returns decision payload and audit artifacts.
- **GET /cds/decisions/{capsule_id}**: Retrieve decision result, risk report, and replay instructions.
- **POST /cds/replay**: Run deterministic replay for a capsule_id; returns replay verification status.
- **GET /cds/health**: Service health and determinism status.
