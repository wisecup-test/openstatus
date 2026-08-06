# Use console.error and structured logging for error tracking in query procedures: Error Logging Occur

These rules are ALWAYS ACTIVE for all query procedures that interact with primary datastores, public and protected API procedures that handle user input validation, service boundary layers that transform domain errors to API errors, cron workflows that execute periodic database operations, and external service integration points that may fail during query execution.

### Rules

- **R-ELO-001** MUST: Error logging MUST occur before error propagation to external error tracking services or error transformation layers.

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

<enforcement>
Code review verification that query procedures include structured error logging before error propagation is mandatory. Integration test coverage that asserts error logging occurs for query failure scenarios is mandatory. Static analysis checks that verify error logging patterns at service boundary layers are mandatory. Claude Code MUST NOT skip or defer verification.
</enforcement>