# Standardize Context-Aware Structured Logging for Error Reporting: Http Client Operations

These rules are ALWAYS ACTIVE for all services that perform error logging in runtime operations, particularly those that interact with external services via HTTP clients.

### Rules

- **R-HTTP-LOG-001** SHOULD: HTTP client operations that interact with external services SHOULD log errors with response status codes and request identifiers.

### Verify

```bash
# Discover the project's code analysis tooling and execute static analysis to identify logging call sites that do not use context-aware logging patterns
# (Tool discovery required from project repository)

# Discover the project's testing framework and run integration tests that verify environment variable validation occurs at startup and produces appropriate error messages for missing configuration
# (Framework discovery required from project repository)

# Discover the project's log aggregation configuration and verify that structured log fields are properly indexed and queryable for error correlation
# (Log aggregation tooling discovery required from project repository)
```

**Accept when:**
- All error logging call sites use context-aware structured logging APIs and preserve request correlation identifiers
- All required environment variables are validated at application startup with clear error messages for missing or invalid values
- Log entries contain sufficient structured fields to enable automated error correlation and analysis across service boundaries

<enforcement>
Claude Code MUST NOT skip or defer verification. Static analysis violations block pull request merge until resolved. Code review findings require remediation before approval. Production incidents caused by inadequate error logging trigger retrospectives and pattern reinforcement.
</enforcement>