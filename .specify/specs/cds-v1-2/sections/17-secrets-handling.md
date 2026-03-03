# Secrets handling
- **Sources**: Vault or cloud secrets manager.
- **Injection method**: environment variables for short-lived tokens.
- **Rotation**: 30 days or on compromise.
- **Invariant**: NO secrets in code, logs, or spec artifacts.
