# Dependency management
- **External systems**: Snapshot store, audit ledger, config signature service, deterministic container registry.
- **Compatibility constraints**: solver version locked per config; container digest pinned in capsule.
- **Failure behavior**: if any dependency fails, system enters safe halt mode.
