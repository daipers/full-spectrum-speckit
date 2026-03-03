# Acceptance criteria
- AC-1 (MUST): For any decision, if Tier 1-3 constraints are infeasible, the output action is reject or resize and no Tier 4-5 optimization is executed.
- AC-2 (MUST): A Decision Capsule can be replayed to produce bitwise-identical outputs when capsule environment constraints are met.
- AC-3 (MUST): Every run emits a feasibility certificate or infeasibility diagnostic with constraint attribution.
- AC-4 (SHOULD): When Mode A fails determinism or feasibility, Mode B is attempted and the report includes coverage gaps.
- AC-5 (MAY): Overrides are accepted only via override capsules with expiry and rollback plan.
