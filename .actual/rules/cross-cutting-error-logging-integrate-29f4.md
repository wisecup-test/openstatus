# Use console.error and structured logging for error tracking in query procedures: Error Logging Integrate

These rules are ALWAYS ACTIVE for all query procedures that interact with primary datastores, public and protected API procedures that handle user input validation, service boundary layers that transform domain errors to API errors, cron workflows that execute periodic database operations, and external service integration points that may fail during query execution.

### Rules

- **R-ELI-001** MUST: Implement error logging immediately after catching exceptions in query procedures, before any error transformation or propagation to external services.
- **R-ELI-002** MUST: Structure error log metadata to include entity type, entity identifier, operation name, and failure reason as separate fields rather than concatenated strings.
- **R-ELI-003** MUST: Use warning-level severity for data inconsistency errors that are filtered rather than blocking operations, and error-level severity for query execution failures.
- **R-ELI-004** SHOULD: Coordinate error logging schema with external error tracking service integration to ensure consistent field naming and metadata structure.
- **R-ELI-005** MAY: Error logging MAY integrate with external error tracking services for aggregation and alerting.

### Verify

```bash
# Discover the project's test execution mechanism and run integration tests
# that verify error logging occurs for query procedure failures
# (Exact command depends on project's test runner — inspect package.json or build config)

# Discover the project's static analysis tooling and verify that query procedures
# include error logging before error propagation
# (Exact command depends on project's linter/analyzer — inspect build config)

# Discover the project's log aggregation configuration and verify that structured
# error metadata fields are correctly indexed
# (Exact command depends on project's logging infrastructure)
```

**Accept when:**
- All query procedures that interact with primary datastores include structured error logging with contextual metadata
- Error logs include entity identifiers, operation type, and failure reason as separate structured fields
- Data inconsistency errors are logged at warning level without blocking operations
- Integration tests verify error logging occurs before error transformation or external service propagation
- Code review verification confirms query procedures include structured error logging before error propagation

<enforcement>
Claude Code MUST NOT skip or defer verification. All query procedures must be reviewed for compliance with R-ELI-001 through R-ELI-005 before code acceptance. Integration tests MUST assert error logging occurs for query failure scenarios. Violations result in code review rejection.
</enforcement>