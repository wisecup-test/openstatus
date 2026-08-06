# Use console.error and structured logging for error tracking in query procedures: Query Procedures That

These rules are ALWAYS ACTIVE for all query procedures that interact with primary datastores, including public and protected API procedures, service boundary layers, and cron-triggered workflows.

### Rules

- **R-QP-001** MUST: All query procedures that interact with primary datastores MUST log errors using structured logging that includes contextual metadata such as entity identifiers, operation type, and failure reason.
- **R-QP-002** MUST: Implement error logging immediately after catching exceptions in query procedures, before any error transformation or propagation to external services.
- **R-QP-003** MUST: Structure error log metadata to include entity type, entity identifier, operation name, and failure reason as separate fields rather than concatenated strings.
- **R-QP-004** MUST: Use warning-level severity for data inconsistency errors that are filtered rather than blocking operations, and error-level severity for query execution failures.
- **R-QP-005** MUST: Coordinate error logging schema with external error tracking service integration to ensure consistent field naming and metadata structure.

### Verify

```bash
# Discover the project's test execution mechanism and run integration tests
# that verify error logging occurs for query procedure failures

# Discover the project's static analysis tooling and verify that query
# procedures include error logging before error propagation

# Discover the project's log aggregation configuration and verify that
# structured error metadata fields are correctly indexed
```

**Accept when:**
- All query procedures that interact with primary datastores include structured error logging with contextual metadata
- Error logs include entity identifiers, operation type, and failure reason as separate structured fields
- Data inconsistency errors are logged at warning level without blocking operations
- Integration tests verify error logging occurs before error transformation or external service propagation
- Error logging occurs immediately after exception handling, before error propagation

<enforcement>
Claude Code MUST NOT skip or defer verification. Code review rejection is required for query procedures that lack structured error logging. Integration test failures for missing error logging in query failure paths must be resolved before merge.
</enforcement>