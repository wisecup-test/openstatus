# Use console.error and structured logging for error tracking in query procedures: Data Inconsistency Errors

These rules are ALWAYS ACTIVE for all query procedures that interact with primary datastores, public and protected API procedures that handle user input validation, service boundary layers that transform domain errors to API errors, cron workflows that execute periodic database operations, and external service integration points that may fail during query execution.

### Rules

- **R-QUERY-001** MUST: Data inconsistency errors MUST be logged with warning-level severity and include sufficient context to identify the inconsistent entity without blocking the operation.
- **R-QUERY-002** MUST: Implement error logging immediately after catching exceptions in query procedures, before any error transformation or propagation to external services.
- **R-QUERY-003** MUST: Structure error log metadata to include entity type, entity identifier, operation name, and failure reason as separate fields rather than concatenated strings.
- **R-QUERY-004** MUST: Use warning-level severity for data inconsistency errors that are filtered rather than blocking operations, and error-level severity for query execution failures.
- **R-QUERY-005** MUST: Coordinate error logging schema with external error tracking service integration to ensure consistent field naming and metadata structure.

### Verify

```bash
# Discover the project's test execution mechanism and run integration tests that verify error logging occurs for query procedure failures
# (Exact command depends on project's build tool and test framework — derive from repository)

# Discover the project's static analysis tooling and verify that query procedures include error logging before error propagation
# (Exact command depends on project's linting and analysis configuration — derive from repository)

# Discover the project's log aggregation configuration and verify that structured error metadata fields are correctly indexed
# (Exact command depends on project's logging infrastructure — derive from repository)
```

**Accept when:**
- All query procedures that interact with primary datastores include structured error logging with contextual metadata
- Error logs include entity identifiers, operation type, and failure reason as separate structured fields
- Data inconsistency errors are logged at warning level without blocking operations
- Integration tests verify error logging occurs before error transformation or external service propagation
- Code review verification confirms query procedures include structured error logging before error propagation
- Static analysis checks verify error logging patterns at service boundary layers

<enforcement>
Claude Code MUST NOT skip or defer verification. All query procedures MUST include structured error logging before error propagation. Violations result in code review rejection, integration test failures, or post-incident review when production errors lack sufficient context for diagnosis.
</enforcement>