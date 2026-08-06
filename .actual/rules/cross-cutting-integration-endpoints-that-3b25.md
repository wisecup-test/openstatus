# Enforce Zod Schema Validation at Integration Boundaries: Integration Endpoints That

These rules are ALWAYS ACTIVE for all integration endpoints that accept external input from HTTP requests, message queues, webhooks, cache deserialization, and service-to-service boundaries.

### Rules

- **R-INT-001** MUST: All integration endpoints that accept external input MUST define explicit schema validators for request payloads before processing business logic.
- **R-INT-002** MUST: Schema validators MUST be defined as constants colocated with endpoint handlers to maintain clear visibility of validation contracts.
- **R-INT-003** MUST: Use safe parsing methods that return result types for validation errors to enable graceful error handling without exception-based control flow.
- **R-INT-004** MUST: Log validation failures with structured error details including the validation library error object to support debugging.
- **R-INT-005** MUST: Sanitize error messages returned to external clients to prevent exposure of internal implementation details or security-sensitive information.
- **R-INT-006** SHOULD: Design schemas to accept optional fields and use permissive validation for non-critical attributes while enforcing strict validation only for security-critical fields.
- **R-INT-007** MAY: Cache compiled schemas where supported by the validation library to mitigate performance overhead in latency-sensitive endpoints.

### Verify

```bash
# Discover and run the project's integration test suite
# covering validation behavior at external boundaries
grep -r "integration" package.json || grep -r "test" package.json

# Discover and run static type checking
# to verify schema definitions provide type inference
grep -r "typecheck\|tsc" package.json

# Discover and run linting configuration
# to ensure schema definitions are colocated with handlers
grep -r "lint" package.json
```

**Accept when:**
- All integration endpoints that accept external input have explicit schema validators defined and applied before business logic execution
- Validation failures are logged with structured error details and result in request rejection with appropriate error responses
- Schema definitions provide type inference that eliminates redundant type annotations in endpoint handler code
- Error messages returned to external clients are sanitized and do not expose internal details

<enforcement>
Claude Code MUST NOT skip or defer verification. All integration endpoints MUST have schema validators present and active before merging.
</enforcement>