# Purpose and scope
- **In scope**: A deterministic, auditable decision workflow that evaluates capital commitments with lexicographic guardrails, WDRO-CVaR primary mode, and scenario-robust fallback.
- **Out of scope**: Trade execution venues, post-trade allocation, portfolio reconciliation, and real-time OMS integration.
- **Environments / blast radius**: Runs in controlled compute environments (prod and staging) and impacts approval/resize/reject decisions for capital commitments. Incorrect outcomes can block commitments or expose the portfolio to breach risk.
