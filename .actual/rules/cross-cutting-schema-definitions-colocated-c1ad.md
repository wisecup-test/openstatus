# Enforce Zod Schema Validation at Integration Boundaries: Schema Definitions Colocated

These rules are ALWAYS ACTIVE for all integration endpoints and service boundaries that accept external input, including HTTP API endpoints, message queue consumers, webhook handlers, cache deserialization, and service-to-service integration points.

### Rules

- **R-SCHEMA-001** SHOULD: Schema definitions SHOULD be colocated with integration endpoint handlers to maintain visibility of validation contracts.
- **R-SCHEMA-002** MUST: Define schema validators as constants colocated with endpoint handlers to maintain clear visibility of validation contracts and enable type inference for downstream code.
- **R-SCHEMA-003** MUST: Use safe parsing methods that return result types for validation errors to enable graceful error handling without exception-based control flow.
- **R-SCHEMA-004** MUST: Log validation failures with structured error details including the validation library error object to support debugging while sanitizing error messages returned to external clients.
- **R-SCHEMA-005** MUST: Apply schema validators before business logic execution for all integration endpoints that accept external input.
- **R-SCHEMA-006** MUST: Reject requests with validation failures and return appropriate error responses.
- **R-SCHEMA-007** SHOULD: Design schemas to accept optional fields and use permissive validation for non-critical attributes while enforcing strict validation only for security-critical fields.
- **R-SCHEMA-008** SHOULD: Implement error sanitization layer that logs detailed validation errors internally while returning generic error messages to external clients.

### Verify

```bash
# Discover the project's test runner from the dependency manifest and execute the integration test suite covering validation behavior at external boundaries
# (Exact command depends on project's build tool — inspect dependency manifest)

# Discover the project's static analysis configuration and execute type checking to verify schema definitions provide type inference for validated payloads
# (Exact command depends on project's static analysis tool — inspect configuration)

# Discover the project's linting configuration and execute validation to ensure schema definitions are colocated with integration endpoint handlers
# (Exact command depends on project's linter — inspect configuration)
```

**Accept when:**
- All integration endpoints that accept external input have explicit schema validators defined and applied before business logic execution
- Validation failures are logged with structured error details and result in request rejection with appropriate error responses
- Schema definitions provide type inference that eliminates redundant type annotations in endpoint handler code
- Schema validators are colocated with their corresponding endpoint handlers
- Safe parsing methods returning result types are used instead of exception-based validation

<enforcement>
Claude Code MUST NOT skip or defer verification. All integration endpoints MUST have schema validators present and colocated with handlers before business logic execution. Validation failures MUST be logged with structured error details and sanitized before external exposure.
</enforcement>