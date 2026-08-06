# Use console.error and structured logging for error tracking in query procedures: Error Logs Include

These rules are ALWAYS ACTIVE for all query procedures that interact with primary datastores, public and protected API procedures that handle user input validation, service boundary layers that transform domain errors to API errors, cron workflows that execute periodic database operations, and external service integration points that may fail during query execution.

### Rules

- **R-ERRLOG-001** SHOULD: Error logs SHOULD include structured fields for domain entities rather than unstructured string concatenation.

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
Clause Code MUST NOT skip or defer verification. Code review rejection for query procedures that lack structured error logging. Integration test failures for missing error logging in query failure paths. Post-incident review when production errors lack sufficient context for diagnosis.
</enforcement>