# Enforce Zod Schema Validation at Integration Boundaries: Integration Endpoints Use

These rules are ALWAYS ACTIVE for all integration endpoints that accept external input from HTTP requests, message queues, webhooks, cache deserialization, and service-to-service boundaries.

### Rules

- **R-INT-001** MAY: Integration endpoints MAY use safe parsing methods that return result types instead of throwing exceptions for validation errors.
- **R-INT-002** MUST: Define schema validators as constants colocated with endpoint handlers to maintain clear visibility of validation contracts and enable type inference for downstream code.
- **R-INT-003** MUST: Apply schema validation before business logic execution for all integration endpoints accepting external input.
- **R-INT-004** MUST: Log validation failures with structured error details including the validation library error object to support debugging while sanitizing error messages returned to external clients.
- **R-INT-005** SHOULD: Use declarative schema definitions with type inference to reduce duplication between validation logic and type definitions.
- **R-INT-006** SHOULD: Design schemas to accept optional fields and use permissive validation for non-critical attributes while enforcing strict validation only for security-critical fields.
- **R-INT-007** SHOULD: Implement error sanitization layer that logs detailed validation errors internally while returning generic error messages to external clients.

### Verify

```bash
# Discover the project's test runner from the dependency manifest and execute the integration test suite covering validation behavior at external boundaries
# (Test runner discovery required from project manifest)

# Discover the project's static analysis configuration and execute type checking to verify schema definitions provide type inference for validated payloads
# (Static analysis tool discovery required from project configuration)

# Discover the project's linting configuration and execute validation to ensure schema definitions are colocated with integration endpoint handlers
# (Linting tool discovery required from project configuration)
```

**Accept when:**
- All integration endpoints that accept external input have explicit schema validators defined and applied before business logic execution
- Validation failures are logged with structured error details and result in request rejection with appropriate error responses
- Schema definitions provide type inference that eliminates redundant type annotations in endpoint handler code
- Schema validators are colocated with their corresponding endpoint handlers
- Validation error messages are sanitized before external exposure while detailed errors are logged internally

<enforcement>
Claude Code MUST NOT skip or defer verification. All integration endpoints MUST have schema validation applied at boundaries. Code review and static analysis MUST block pull requests adding integration endpoints without schema validation.
</enforcement>