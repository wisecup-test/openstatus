# Enforce Zod Schema Validation at Integration Boundaries: Validation Failures Logged

These rules are ALWAYS ACTIVE for all HTTP API endpoints, message queue consumers, webhook handlers, cache deserialization operations, and service-to-service integration boundaries that accept external input.

### Rules

- **R-VAL-001** MUST: Validation failures MUST be logged with structured error details and MUST reject the request before business logic execution.
- **R-VAL-002** MUST: Define schema validators as constants colocated with endpoint handlers to maintain clear visibility of validation contracts and enable type inference for downstream code.
- **R-VAL-003** MUST: Use safe parsing methods that return result types for validation errors to enable graceful error handling without exception-based control flow.
- **R-VAL-004** MUST: Log validation failures with structured error details including the validation library error object to support debugging while sanitizing error messages returned to external clients.
- **R-VAL-005** SHOULD: Design schemas to accept optional fields and use permissive validation for non-critical attributes while enforcing strict validation only for security-critical fields.
- **R-VAL-006** MAY: Exception EX-001 permits performance-critical hot paths where input has been pre-validated by upstream middleware to defer schema validation, provided alternative validation mechanisms are documented and equivalent test coverage is maintained.

### Verify

```bash
# Discover the project's test runner from the dependency manifest and execute the integration test suite covering validation behavior at external boundaries
# (Test runner discovery and execution command to be derived from project repository)

# Discover the project's static analysis configuration and execute type checking to verify schema definitions provide type inference for validated payloads
# (Static analysis tool discovery and execution command to be derived from project repository)

# Discover the project's linting configuration and execute validation to ensure schema definitions are colocated with integration endpoint handlers
# (Linting tool discovery and execution command to be derived from project repository)
```

**Accept when:**
- All integration endpoints that accept external input have explicit schema validators defined and applied before business logic execution
- Validation failures are logged with structured error details and result in request rejection with appropriate error responses
- Schema definitions provide type inference that eliminates redundant type annotations in endpoint handler code
- Validation error messages are sanitized before external exposure while detailed errors are logged internally

<enforcement>
Claude Code MUST NOT skip or defer verification. All integration endpoints must be audited for schema validator presence. Validation failures must be confirmed to log structured error details and reject requests before business logic execution. Static analysis and integration tests must pass before accepting changes.
</enforcement>