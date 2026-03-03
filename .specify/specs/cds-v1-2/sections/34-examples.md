# Examples

## Golden Path Example
Input: Mode A request with valid snapshots and signatures.
Expected output: decision action "accept" with execution plan, risk report with CVaR/ruin/liquidity minima, audit artifacts containing capsule_id, and events CAPSULE_CREATED and SOLVER_FEASIBLE.

## Failure Path Example
Input: Mode A request where liquidity cascade fails to converge.
Expected output: decision action "reject" or "halt", risk report with conservative loss bound, audit event CASCADE_NONCONVERGENCE, and escalation notification triggered.
