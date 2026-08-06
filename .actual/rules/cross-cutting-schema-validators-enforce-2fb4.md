# Enforce Zod Schema Validation at Integration Boundaries: Schema Validators Enforce

These rules are ALWAYS ACTIVE for all integration endpoints and service boundaries that accept external input from HTTP requests, message queues, webhooks, or deserialization of persisted state.

### Rules

- **R-ZOD-001** MUST: Schema validators MUST enforce type constraints, required fields, and format validation for all structured data crossing integration boundaries (HTTP API endpoints, message queue consumers, webhook handlers, cache deserialization, service-to-service boundaries).
- **R-ZOD-002** MUST: Define schema validators as constants colocated with endpoint handlers to maintain clear visibility of validation contracts and enable type inference for downstream code.
- **R-ZOD-003** MUST: Use safe parsing methods that return result types for validation errors to enable graceful error handling without exception-based control flow.
- **R-ZOD-004** MUST: Log validation failures with structured error details including the validation library error object to support debugging while sanitizing error messages returned to external clients.
- **R-ZOD-005** SHOULD: Design schemas to accept optional fields and use permissive validation for non-critical attributes while enforcing strict validation only for security-critical fields to avoid brittleness with backward-compatible external changes.
- **R-ZOD-006** MAY: Cache compiled schemas where supported by the validation library to mitigate performance overhead on latency-sensitive integration endpoints under high load.

### Verify

```bash
# Discover and run the project's integration test suite covering validation behavior
# at external boundaries (valid and invalid payloads)

# Discover and run the project's static type checker to verify schema definitions
# provide type inference for validated payloads

# Discover and run the project's linter to ensure schema definitions are colocated
# with integration endpoint handlers

# Verify no integration endpoints accept external input without explicit schema validators
```

**Accept when:**
- All integration endpoints that accept external input have explicit schema validators defined and applied before business logic execution
- Validation failures are logged with structured error details and result in request rejection with appropriate error responses
- Schema definitions provide type inference that eliminates redundant type annotations in endpoint handler code
- Schema validators are colocated with their corresponding endpoint handlers

<enforcement>
Claude Code MUST NOT skip or defer verification. All integration endpoints accepting external input MUST have schema validators present and active before business logic execution. Code review and static analysis MUST block pull requests that add integration endpoints without schema validation.
</enforcement>