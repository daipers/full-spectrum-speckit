# Postconditions

## On Success
- Decision and risk report are persisted with capsule linkage and audit events.

## On Failure
- Decision result is "halt" with reason, and audit log includes failure event.

## On Cancel
- Partial artifacts are marked abandoned; no decision is emitted.
