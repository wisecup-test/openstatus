# Standardize Context-Aware Structured Logging for Error Reporting: Runtime Configuration Values

These rules are ALWAYS ACTIVE for all services that perform error logging in runtime operations, including all services that perform HTTP client operations to external APIs, all error handling code paths in service handlers and background workers, and all operations that retrieve configuration from environment variables.

### Rules

- **R-CTXLOG-001** MUST: Runtime configuration values required for external service integration MUST be retrieved from environment variables.

### Verify

```bash
# Discover the project's code analysis tooling and execute static analysis to identify logging call sites that do not use context-aware logging patterns
# Discover the project's testing framework and run integration tests that verify environment variable validation occurs at startup and produces appropriate error messages for missing configuration
# Discover the project's log aggregation configuration and verify that structured log fields are properly indexed and queryable for error correlation
```

**Accept when:**
- All error logging call sites use context-aware structured logging APIs and preserve request correlation identifiers
- All required environment variables are validated at application startup with clear error messages for missing or invalid values
- Log entries contain sufficient structured fields to enable automated error correlation and analysis across service boundaries

<enforcement>
Claude Code MUST NOT skip or defer verification. Static analysis violations block pull request merge until resolved. Code review findings require remediation before approval. Production incidents caused by inadequate error logging trigger retrospectives and pattern reinforcement.
</enforcement>