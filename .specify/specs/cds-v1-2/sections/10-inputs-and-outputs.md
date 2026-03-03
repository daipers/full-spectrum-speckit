# Inputs and outputs

## Inputs
- Input schema (JSON Schema Draft 2020-12):
```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "type": "object",
  "required": ["portfolio_state", "market_state", "commitment_candidate", "governed_parameters", "data_snapshots"],
  "properties": {
    "portfolio_state": { "type": "object", "description": "Positions, exposures, collateral, cash, credit lines, limits." },
    "market_state": { "type": "object", "description": "Prices, vols, correlations, liquidity metrics, regime flags." },
    "commitment_candidate": { "type": "object", "description": "Size, tenor, hedges, execution schedule, collateral terms." },
    "governed_parameters": { "type": "object", "description": "delta, T, floors, epsilon controls, kappa bounds, limits." },
    "data_snapshots": {
      "type": "object",
      "required": ["portfolio_snapshot_ref", "market_snapshot_ref"],
      "properties": {
        "portfolio_snapshot_ref": { "type": "string" },
        "market_snapshot_ref": { "type": "string" }
      }
    },
    "mode": { "type": "string", "enum": ["A", "B", "C"], "default": "A" }
  }
}
```
- Example input:
```json
{
  "portfolio_state": { "cash": 25000000, "positions": "..." },
  "market_state": { "prices": "...", "liquidity": "..." },
  "commitment_candidate": { "size": 5000000, "tenor_days": 90 },
  "governed_parameters": { "delta": 0.01, "T_days": 90, "epsilon": 0.25 },
  "data_snapshots": { "portfolio_snapshot_ref": "sha256:...", "market_snapshot_ref": "sha256:..." },
  "mode": "A"
}
```

## Outputs
- Output schema:
```json
{
  "type": "object",
  "required": ["decision", "risk_report", "audit_artifacts"],
  "properties": {
    "decision": {
      "type": "object",
      "properties": {
        "action": { "type": "string", "enum": ["accept", "reject", "resize", "renegotiate", "halt"] },
        "execution_plan": { "type": "object" }
      }
    },
    "risk_report": { "type": "object", "description": "CVaR, ruin probability, liquidity minima, stress losses, certificates." },
    "audit_artifacts": { "type": "object", "description": "Capsule hash, signed config, solver logs, replay instructions." }
  }
}
```
- Example output:
```json
{
  "decision": { "action": "resize", "execution_plan": { "size": 3200000, "slices": 6 } },
  "risk_report": { "cvar": 0.075, "ruin_prob": 0.006, "liquidity_min": 12000000 },
  "audit_artifacts": { "capsule_id": "sha256:...", "replay": "capsule://..." }
}
```
