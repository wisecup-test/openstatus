# Use console.error and structured logging for error tracking in query procedures: Query Procedures Log

These rules are ALWAYS ACTIVE for all query procedures that interact with primary datastores, including public and protected API procedures, cron-triggered workflows, and external service integration points.

### Rules

- **R-QPL-001** SHOULD: Query procedures SHOULD log errors at the service boundary layer before transforming errors for API consumers.

### Verify

```bash
# Discover the project's test execution mechanism and run integration tests that verify error logging occurs for query procedure failures
# Discover the project's static analysis tooling and verify that query procedures include error logging before error propagation
# Discover the project's log aggregation configuration and verify that structured error metadata fields are correctly indexed
```

**Accept when:**
- All query procedures that interact with primary datastores include structured error logging with contextual metadata
- Error logs include entity identifiers, operation type, and failure reason as separate structured fields
- Data inconsistency errors are logged at warning level without blocking operations
- Integration tests verify error logging occurs before error transformation or external service propagation
- Error logging is implemented immediately after catching exceptions, before any error transformation or propagation
- Error log metadata is structured with entity type, entity identifier, operation name, and failure reason as separate fields

<enforcement>
Claude Code MUST NOT skip or defer verification. Code review rejection is required for query procedures that lack structured error logging. Integration test failures for missing error logging in query failure paths must be resolved before merge.
</enforcement>