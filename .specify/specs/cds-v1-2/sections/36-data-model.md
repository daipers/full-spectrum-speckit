# Data model
- **Core entities**: Decision Capsule, governed config, snapshot references, scenario catalog, liquidity path set, feasibility certificate, audit event.
- **Identifiers**: content-addressed hashes for capsules and snapshots; request_hash for intake.
- **Relationships**: Decision Capsule links to snapshots, configs, solver logs, and audit events; risk report references capsule_id.
- **Retention**: immutable artifacts; append-only audit ledger.
