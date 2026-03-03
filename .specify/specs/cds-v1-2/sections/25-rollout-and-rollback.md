# Rollout and rollback

## Rollout Plan
- **Strategy**: staged rollout with canary decisions.
- **Stages**: staging -> canary -> production.
- **Feature flags**: mode A/B selection, override pathway.

## Rollback Triggers
- determinism mismatch across environments
- sustained Tier 1 constraint breach rate increase

## Rollback Steps
- revert to previous signed config and container digest
- disable new decisions and require manual review
